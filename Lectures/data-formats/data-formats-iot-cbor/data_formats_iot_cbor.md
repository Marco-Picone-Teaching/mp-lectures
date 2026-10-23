<!-- omit in toc -->
# Concise Binary Object Representation (CBOR)

<!-- omit in toc -->
## Lecture Information

| **Lecture Title**   | Concise Binary Object Representation (CBOR)                                      |
|---------------------|----------------------------------------------------------------------------------|
| **Author**          | Prof. Marco Picone (marco.picone@unimore.it)                                     |
| **License**         | [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/) | 


<!-- omit in toc -->
# Table of Contents

- [A. What is CBOR](#a-what-is-cbor)
- [B. Why a Binary Format](#b-why-a-binary-format)
- [C. Data Model \& Types](#c-data-model--types)
- [D. Encoding Structure](#d-encoding-structure)
  - [D.1 The Initial Byte](#d1-the-initial-byte)
  - [D.2 Floats \& Simple Values](#d2-floats--simple-values)
- [E. Where CBOR is Used in IoT](#e-where-cbor-is-used-in-iot)
- [F. Python: the cbor2 Library](#f-python-the-cbor2-library)
  - [F.1 Encode and Decode](#f1-encode-and-decode)
  - [F.2 Read and Write CBOR Files](#f2-read-and-write-cbor-files)
- [G. Size Comparison with JSON](#g-size-comparison-with-json)
- [H. Looking Inside a CBOR Message](#h-looking-inside-a-cbor-message)
- [I. Example: SenML in CBOR](#i-example-senml-in-cbor)
- [J. Common Pitfalls](#j-common-pitfalls)
- [References](#references)

# A. What is CBOR

**CBOR (Concise Binary Object Representation)** is a **binary serialization format** whose data model is a **superset of JSON**: everything that can be represented in JSON can be represented in CBOR, plus binary data, more number types and extensible tags (e.g., dates).

- **Specification:** [RFC 8949](https://www.rfc-editor.org/rfc/rfc8949) and [cbor.io](https://cbor.io/)
- **Media type:** `application/cbor` (CoAP Content-Format `60`)
- **File extension:** `.cbor`

CBOR can be thought of as a kind of **"binary JSON"**: the structure of the data is largely the same (maps, arrays, strings, numbers, booleans, null), while the **encoding on the wire** changes.

**Design Goals (RFC 8949):**

- **Small code size:** encoders and decoders can fit in very constrained devices.
- **Compact messages:** reasonably small serialization, without the need for compression.
- **Schema-less:** data can be decoded without a schema (unlike, e.g., Protocol Buffers).
- **Extensibility:** new data types can be added through **tags** without breaking existing decoders.

---

# B. Why a Binary Format

JSON is text: the number `1727535600` takes 10 bytes (one per digit), every key is repeated as a quoted string, and the receiver needs to parse characters to rebuild numbers. On a battery-powered sensor transmitting over a low-power radio network (e.g., LoRaWAN, NB-IoT, 6LoWPAN), **bytes and CPU cycles can matter a lot**.

**Advantages:**

- **Smaller messages** (in many cases): numbers are stored in binary using few bytes, small integers fit in one byte.
- **Typically faster and simpler parsing:** each item starts with a header byte describing its type and length, so there is less need to scan characters, escape strings or convert text to numbers. Encoders/decoders can fit in a few KB of firmware.
- **Native binary data** (byte strings) without base64 encoding (which adds about 33% overhead in JSON).
- **Schema-less** like JSON: there is no need to define and compile a schema.

**Drawback:**

- It is **not human-readable**, so a tool is usually needed to inspect it (e.g., [cbor.me](https://cbor.me/)).

---

# C. Data Model & Types

| CBOR Major Type | Description | JSON Equivalent | Python Type (`cbor2`) |
|-----------------|-------------|-----------------|------------------------|
| 0 / 1 | Unsigned / negative integer | number | `int` |
| 2 | Byte string | – (base64 string) | `bytes` |
| 3 | Text string (UTF-8) | string | `str` |
| 4 | Array | array | `list` |
| 5 | Map | object | `dict` |
| 6 | Tag (semantic meaning, e.g., date) | – | e.g., `datetime` |
| 7 | Float, `true`, `false`, `null` | number, boolean, null | `float`, `bool`, `None` |

Unlike JSON, CBOR **map keys can be any type**, including integers. This is used by many IoT standards (**SenML-CBOR**, **COSE**, **CWT**) to replace long string keys with small integers.

> **Note:** The structure of CBOR data can be described with **CDDL** (Concise Data Definition Language, RFC 8610), which plays for CBOR a role similar to the one of JSON Schema for JSON.

---

# D. Encoding Structure

## D.1 The Initial Byte

Every CBOR data item starts with an **initial byte** composed of two fields:

```
  7   6   5   4   3   2   1   0
+-----------+-------------------+
| Major Type| Additional Info   |
|  (3 bits) |     (5 bits)      |
+-----------+-------------------+
```

- **Major Type (3 bits):** the type of the data item (0-7, see [C. Data Model & Types](#c-data-model--types)).
- **Additional Info (5 bits):** depending on its value, it contains the value itself or tells how many bytes follow:

| Additional Info | Meaning |
|-----------------|---------|
| 0-23 | The value (or the length) is **directly encoded** in the initial byte |
| 24 | The value follows in the next **1 byte** |
| 25 | The value follows in the next **2 bytes** |
| 26 | The value follows in the next **4 bytes** |
| 27 | The value follows in the next **8 bytes** |
| 31 | **Indefinite length** (for strings, arrays and maps; terminated by `0xff`) |

For integers the "value" is the number itself; for strings, arrays and maps it is the **length** (number of bytes, elements or key/value pairs).

**Examples:**

| Data | Encoding | Explanation |
|------|----------|-------------|
| `10` | `0a` | major type 0, value 10 (fits in the 5 bits) |
| `500` | `19 01 f4` | major type 0, additional info 25 → 2 bytes follow (`0x01f4` = 500) |
| `-1` | `20` | major type 1 (negative), encoded value `-1 - 0` |
| `"v"` | `61 76` | major type 3 (text), length 1, followed by the UTF-8 byte of `v` |
| `[1, 2]` | `82 01 02` | major type 4 (array), 2 elements |
| `{"v": 1}` | `a1 61 76 01` | major type 5 (map), 1 key/value pair |

> **Note:** As in the CoAP header, the first bits of each byte immediately tell the decoder how to interpret what follows: this is what makes CBOR easy to parse on constrained devices.

---

## D.2 Floats & Simple Values

Major type 7 encodes floating-point numbers and simple values:

| Initial Byte | Meaning |
|--------------|---------|
| `f4` | `false` |
| `f5` | `true` |
| `f6` | `null` |
| `f9` | half-precision float (16 bits) follows |
| `fa` | single-precision float (32 bits) follows |
| `fb` | double-precision float (64 bits) follows |

An encoder can choose the **smallest float size** that represents the value without loss of precision (e.g., `21.5` fits in a 16-bit half-precision float: `f9 4d 60`).

---

# E. Where CBOR is Used in IoT

- **CoAP** payloads on constrained devices (`application/cbor`, `application/senml+cbor`).
- **SenML-CBOR** sensor measurements (see [I. Example: SenML in CBOR](#i-example-senml-in-cbor)).
- **LwM2M** device management (SenML-CBOR content format).
- **Security:** COSE (CBOR Object Signing and Encryption) and CWT (CBOR Web Token, the compact version of JWT).
- **WebAuthn / FIDO2** (passkeys) uses CBOR for authenticator data.

---

# F. Python: the cbor2 Library

```bash
pip install "cbor2>=5.6,<6"
```

> **Note:** The 6.x versions of `cbor2` are built in Rust: if no pre-built package is available for your platform, the installation requires a Rust compiler. The 5.x series works everywhere and has the same API used here.

The API mirrors the `json` module, but works with **bytes** instead of strings:

| Function | From | To |
|----------|------|----|
| `cbor2.load(f)` | CBOR file (opened in `rb`) | Python object |
| `cbor2.dump(obj, f)` | Python object | CBOR file (opened in `wb`) |
| `cbor2.loads(data)` | `bytes` | Python object |
| `cbor2.dumps(obj)` | Python object | `bytes` |

---

## F.1 Encode and Decode

```python
import cbor2

measurement = {"device_id": "temp-sensor-01", "value": 21.5, "unit": "C",
               "timestamp": 1727535600000, "online": True}

payload = cbor2.dumps(measurement)        # dict -> bytes (ready to be sent with CoAP/MQTT)
print(type(payload), len(payload))        # Output: <class 'bytes'> 75

decoded = cbor2.loads(payload)            # bytes -> dict
print(decoded == measurement)             # Output: True
print(decoded["value"])                   # Output: 21.5
```

CBOR natively supports **binary data** and **dates**:

```python
import cbor2
from datetime import datetime, timezone

firmware_chunk = {
    "device_id": "temp-sensor-01",
    "chunk": 3,
    "data": bytes([0xDE, 0xAD, 0xBE, 0xEF]),                      # raw bytes, no base64 needed
    "created": datetime(2024, 9, 28, 15, 0, tzinfo=timezone.utc),  # encoded with a CBOR date tag
}

decoded = cbor2.loads(cbor2.dumps(firmware_chunk))
print(decoded["data"])      # Output: b'\xde\xad\xbe\xef'
print(decoded["created"])   # Output: 2024-09-28 15:00:00+00:00
```

---

## F.2 Read and Write CBOR Files

CBOR is binary, so files are opened in **binary mode** (`wb` / `rb`):

```python
import cbor2

measurements = [
    {"device_id": "temp-sensor-01", "value": 21.5, "timestamp": 1727535600},
    {"device_id": "temp-sensor-01", "value": 21.7, "timestamp": 1727535601},
]

with open("measurements.cbor", "wb") as f:
    cbor2.dump(measurements, f)

with open("measurements.cbor", "rb") as f:
    loaded = cbor2.load(f)

print(loaded[1]["value"])   # Output: 21.7
```

---

# G. Size Comparison with JSON

```python
import json
import cbor2

measurement = {"device_id": "temp-sensor-01", "value": 21.5, "unit": "C",
               "timestamp": 1727535600000, "online": True}

json_bytes = json.dumps(measurement, separators=(",", ":")).encode("utf-8")   # compact JSON
cbor_bytes = cbor2.dumps(measurement)
cbor_canonical = cbor2.dumps(measurement, canonical=True)   # smallest float size + sorted keys

print("JSON :", len(json_bytes), "bytes")       # Output: JSON : 94 bytes
print("CBOR :", len(cbor_bytes), "bytes")       # Output: CBOR : 75 bytes
print("CBOR (canonical):", len(cbor_canonical), "bytes")   # Output: CBOR (canonical): 69 bytes
```

The saving here is about 20-25%, because a large part of the message is made of **text** (keys and the device id), which takes the same space in both formats. The gain usually becomes larger when:

- Messages contain mostly **numbers** or **binary data**.
- String keys are replaced by **integer keys** (as SenML-CBOR does).
- The same structure is repeated many times (time series).

As another example, a small device configuration (device id, enabled flag, broker host and port, list of units) takes **77 bytes** in CBOR against **105 bytes** of compact JSON.

> **Note:** CBOR can be combined with compression, but on very small messages (tens of bytes) compression is usually not very effective, while CBOR can still reduce the size.

---

# H. Looking Inside a CBOR Message

Let's encode a tiny message and print the bytes in hexadecimal:

```python
import cbor2

print(cbor2.dumps({"v": 21.5, "t": 1727535600}).hex(" "))
# Output: a2 61 76 fb 40 35 80 00 00 00 00 00 61 74 1a 66 f8 19 f0
```

| Bytes | Meaning |
|-------|---------|
| `a2` | map (major type 5) with 2 pairs |
| `61 76` | text string of length 1: `"v"` |
| `fb 40 35 80 00 00 00 00 00` | 64-bit float: `21.5` |
| `61 74` | text string of length 1: `"t"` |
| `1a 66 f8 19 f0` | 32-bit unsigned integer: `1727535600` |

The message is **19 bytes** against the 25 bytes of the compact JSON `{"v":21.5,"t":1727535600}`.

With `canonical=True` the keys are sorted and the float `21.5` is encoded as a 16-bit half-precision float (`f9 4d 60`, 3 bytes instead of 9), so the message becomes **13 bytes**:

```python
print(cbor2.dumps({"v": 21.5, "t": 1727535600}, canonical=True).hex(" "))
# Output: a2 61 74 1a 66 f8 19 f0 61 76 f9 4d 60
```

> **Tip:** The hex string can be pasted on [cbor.me](https://cbor.me/) to decode it interactively.

---

# I. Example: SenML in CBOR

**SenML-CBOR** (RFC 8428, Section 6) uses the same Records as SenML-JSON, but replaces the field names with **integer labels** (`bn` → `-2`, `bt` → `-3`, `n` → `0`, `u` → `1`, `v` → `2`, ...):

```python
import json
import cbor2

SENML_CBOR_LABELS = {"bver": -1, "bn": -2, "bt": -3, "bu": -4, "bv": -5, "bs": -6,
                     "n": 0, "u": 1, "v": 2, "vs": 3, "vb": 4, "s": 5, "t": 6, "ut": 7, "vd": 8}
SENML_JSON_LABELS = {label: name for name, label in SENML_CBOR_LABELS.items()}

def senml_to_cbor(pack):
    return cbor2.dumps([{SENML_CBOR_LABELS[k]: v for k, v in record.items()} for record in pack])

def senml_from_cbor(data):
    return [{SENML_JSON_LABELS[k]: v for k, v in record.items()} for record in cbor2.loads(data)]

pack = [
    {"bn": "urn:dev:mac:0024befffe804ff1:", "bt": 1727535600, "n": "temperature", "u": "Cel", "v": 21.5},
    {"n": "humidity", "u": "%RH", "v": 48.7},
]

senml_json = json.dumps(pack, separators=(",", ":")).encode("utf-8")
senml_cbor = senml_to_cbor(pack)

print("SenML-JSON:", len(senml_json), "bytes")   # Output: SenML-JSON: 129 bytes
print("SenML-CBOR:", len(senml_cbor), "bytes")   # Output: SenML-CBOR: 94 bytes
print(senml_from_cbor(senml_cbor) == pack)       # Output: True
```

> **Note:** SenML-CBOR (`application/senml+cbor`, Content-Format `112`) is a **different media type** than generic CBOR (`application/cbor`, `60`): the receiver needs to know that the integer keys are SenML labels.

---

# J. Common Pitfalls

- Files are opened in **binary** mode (`rb`/`wb`); CBOR data is `bytes`, not `str`.
- CBOR is **not human-readable**: logging the decoded object (or the hex dump) usually helps when debugging.
- Not every CBOR value can be converted to JSON (byte strings, integer keys, tags): conversions need some care.
- Both sides need to agree on **conventions** (e.g., integer key mapping, date representation), just like with JSON.
- For tiny devices it is worth checking which features are supported by the embedded library (e.g., floats, tags, indefinite lengths).

---

# References

- C. Bormann, P. Hoffman, *Concise Binary Object Representation (CBOR)*, RFC 8949, IETF, December 2020 - [https://www.rfc-editor.org/rfc/rfc8949](https://www.rfc-editor.org/rfc/rfc8949)
- H. Birkholz, C. Vigano, C. Bormann, *Concise Data Definition Language (CDDL)*, RFC 8610, IETF, June 2019 - [https://www.rfc-editor.org/rfc/rfc8610](https://www.rfc-editor.org/rfc/rfc8610)
- C. Jennings, Z. Shelby, J. Arkko, A. Keränen, C. Bormann, *Sensor Measurement Lists (SenML)*, RFC 8428, IETF, August 2018 - [https://www.rfc-editor.org/rfc/rfc8428](https://www.rfc-editor.org/rfc/rfc8428)
- CBOR website - [https://cbor.io/](https://cbor.io/)
- CBOR playground (online decoder) - [https://cbor.me/](https://cbor.me/)
- cbor2 Python library - [https://pypi.org/project/cbor2/](https://pypi.org/project/cbor2/)
