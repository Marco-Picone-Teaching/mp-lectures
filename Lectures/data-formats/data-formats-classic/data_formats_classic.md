<!-- omit in toc -->
# Classic Data Formats: JSON, YAML, TOML & CSV

<!-- omit in toc -->
## Lecture Information

| **Lecture Title**   | Classic Data Formats: JSON, YAML, TOML & CSV                                     |
|---------------------|----------------------------------------------------------------------------------|
| **Author**          | Prof. Marco Picone (marco.picone@unimore.it)                                     |
| **License**         | [Creative Commons Attribution 4.0](https://creativecommons.org/licenses/by/4.0/) | 


<!-- omit in toc -->
# Table of Contents

- [A. Data Representation in IoT \& CPS](#a-data-representation-in-iot--cps)
  - [A.1 Why Data Representation Matters](#a1-why-data-representation-matters)
  - [A.2 Formats Overview](#a2-formats-overview)
  - [A.3 Python Requirements](#a3-python-requirements)
- [B. JSON – JavaScript Object Notation](#b-json--javascript-object-notation)
  - [B.1 What is JSON](#b1-what-is-json)
  - [B.2 Basic components of JSON](#b2-basic-components-of-json)
  - [B.3 Where JSON is used in IoT](#b3-where-json-is-used-in-iot)
    - [B.3.1 Data structures (device descriptions, digital twins)](#b31-data-structures-device-descriptions-digital-twins)
    - [B.3.2 Messages (commands and events)](#b32-messages-commands-and-events)
    - [B.3.3 Telemetry (measurements)](#b33-telemetry-measurements)
    - [B.3.4 REST / HTTP APIs](#b34-rest--http-apis)
  - [B.4 Python: the json module](#b4-python-the-json-module)
  - [B.5 Load JSON data from a file](#b5-load-json-data-from-a-file)
  - [B.6 Dump data to a JSON file](#b6-dump-data-to-a-json-file)
  - [B.7 JSON strings and dictionaries](#b7-json-strings-and-dictionaries)
  - [B.8 Formatting options](#b8-formatting-options)
  - [B.9 Serializing custom objects and dates](#b9-serializing-custom-objects-and-dates)
  - [B.10 Handling invalid JSON](#b10-handling-invalid-json)
  - [B.11 Common pitfalls](#b11-common-pitfalls)
- [C. YAML – YAML Ain't Markup Language](#c-yaml--yaml-aint-markup-language)
  - [C.1 What is YAML](#c1-what-is-yaml)
  - [C.2 Basic syntax](#c2-basic-syntax)
  - [C.3 Advanced features](#c3-advanced-features)
  - [C.4 Where YAML is used in IoT](#c4-where-yaml-is-used-in-iot)
  - [C.5 Python: the PyYAML library](#c5-python-the-pyyaml-library)
  - [C.6 Load a YAML configuration file](#c6-load-a-yaml-configuration-file)
  - [C.7 Write a YAML file](#c7-write-a-yaml-file)
  - [C.8 Multiple documents in one file](#c8-multiple-documents-in-one-file)
  - [C.9 Converting between YAML and JSON](#c9-converting-between-yaml-and-json)
  - [C.10 Common pitfalls](#c10-common-pitfalls)
- [D. TOML – Tom's Obvious, Minimal Language](#d-toml--toms-obvious-minimal-language)
  - [D.1 What is TOML](#d1-what-is-toml)
  - [D.2 Syntax](#d2-syntax)
  - [D.3 Where TOML is used](#d3-where-toml-is-used)
  - [D.4 Python: read TOML with tomllib](#d4-python-read-toml-with-tomllib)
  - [D.5 Python: write TOML with tomli-w](#d5-python-write-toml-with-tomli-w)
  - [D.6 TOML vs YAML for configuration](#d6-toml-vs-yaml-for-configuration)
  - [D.7 Common pitfalls](#d7-common-pitfalls)
- [E. CSV – Comma-Separated Values](#e-csv--comma-separated-values)
  - [E.1 What is CSV](#e1-what-is-csv)
  - [E.2 Syntax](#e2-syntax)
  - [E.3 Where CSV is used in IoT](#e3-where-csv-is-used-in-iot)
  - [E.4 Python: the csv module](#e4-python-the-csv-module)
  - [E.5 Write a CSV file](#e5-write-a-csv-file)
  - [E.6 Read a CSV file](#e6-read-a-csv-file)
  - [E.7 Append measurements to a log](#e7-append-measurements-to-a-log)
  - [E.8 From JSON messages to CSV](#e8-from-json-messages-to-csv)
  - [E.9 Analysis with pandas (optional)](#e9-analysis-with-pandas-optional)
  - [E.10 Common pitfalls](#e10-common-pitfalls)
- [F. Data Validation](#f-data-validation)
  - [F.1 Why validate data](#f1-why-validate-data)
  - [F.2 Three levels of validation](#f2-three-levels-of-validation)
  - [F.3 Where to validate](#f3-where-to-validate)
  - [F.4 JSON Schema](#f4-json-schema)
  - [F.5 Main JSON Schema keywords](#f5-main-json-schema-keywords)
  - [F.6 Python: the jsonschema library](#f6-python-the-jsonschema-library)
  - [F.7 Validating JSON telemetry messages](#f7-validating-json-telemetry-messages)
  - [F.8 Reporting all the errors](#f8-reporting-all-the-errors)
  - [F.9 Validating a YAML configuration](#f9-validating-a-yaml-configuration)
  - [F.10 Writing the schema in YAML](#f10-writing-the-schema-in-yaml)
  - [F.11 Beyond JSON Schema: typed models](#f11-beyond-json-schema-typed-models)
  - [F.12 Best practices](#f12-best-practices)
- [G. Formats Comparison – Which Format for Which Job](#g-formats-comparison--which-format-for-which-job)
  - [G.1 The same configuration in different formats](#g1-the-same-configuration-in-different-formats)
  - [G.2 The same measurements in different formats](#g2-the-same-measurements-in-different-formats)
  - [G.3 Feature comparison](#g3-feature-comparison)
  - [G.4 Decision guide](#g4-decision-guide)
  - [G.5 Python cheat sheet](#g5-python-cheat-sheet)
- [References](#references)

# A. Data Representation in IoT & CPS

This lecture is an overview of the most common **general-purpose data representation formats** (**JSON**, **YAML**, **TOML** and **CSV**): how they work, what they are typically used for and how to read and write them in **Python**. It also introduces **data validation** with JSON Schema and compares the formats with each other.

The focus and the examples are designed for **Internet of Things (IoT)** and **Cyber-Physical Systems (CPS)**, where the way data is represented plays a key role: sensors, actuators, gateways, MQTT/CoAP messages, telemetry and HTTP APIs.

At the same time, the lecture also covers formats that are mainly used for other purposes, such as **configuration files**, **logs and datasets** and **application settings**, which are part of almost any software system.

---

## A.1 Why Data Representation Matters

IoT and Cyber-Physical Systems connect the **physical world** (sensors measuring temperature, energy, position; actuators switching lights or moving machines) with **software** running on devices, gateways and the cloud. Every measurement, command, event and configuration crossing this boundary needs to be represented in a way that all the involved components can interpret correctly: a wrong unit, an ambiguous timestamp or a misread field can lead to wrong decisions with real effects on the physical environment.

These systems are also usually made of very heterogeneous components: firmware written in C on a microcontroller, a gateway in Python, a cloud service in Java or Go, a dashboard in JavaScript. To work together they need to agree on **how data is represented** when it is stored in a file or sent over the network (*serialization*). How data is represented therefore commonly affects interoperability, efficiency on constrained devices and networks, maintainability of configurations and, ultimately, the reliability of the whole system.

Different formats make different trade-offs:

- **Human-readable** (e.g., configuration edited by people) vs. **compact and fast** (e.g., messages from battery-powered devices).
- **Flexible** (each project defines its own structure) vs. **standardized** (different vendors share the same fields).
- **Typed and nested** structures vs. **flat tables**.

Two formats cover a large part of the typical needs:

- **JSON** is commonly used for data that **moves between machines**: data structures, messages, commands and events, telemetry, REST APIs.
- **YAML** is commonly used for data that **configures systems** and is written by humans: device and gateway configuration, docker-compose files, CI pipelines, deployment manifests.

Other formats are often chosen for more specific needs: **TOML** is a frequent alternative to YAML for application settings and **CSV** is a simple option for tabular logs and datasets. IoT-specific formats, such as **SenML** (a standard representation for sensor measurements) and **CBOR** (a compact binary format, similar to JSON, designed with constrained devices in mind), address the requirements of constrained devices and networks and are mentioned in the comparison at the end of the lecture.

---

## A.2 Formats Overview

| Section | Format | Type | Typical IoT use | Python library |
|---------|--------|------|-----------------|----------------|
| [B](#b-json--javascript-object-notation) | JSON | text | data structures, messages, telemetry, REST APIs | `json` (built-in) |
| [C](#c-yaml--yaml-aint-markup-language) | YAML | text | configuration files, docker-compose, CI/CD | `PyYAML` |
| [D](#d-toml--toms-obvious-minimal-language) | TOML | text | application settings, `pyproject.toml` | `tomllib` (built-in), `tomli-w` |
| [E](#e-csv--comma-separated-values) | CSV | text | telemetry logs, datasets, bulk import/export | `csv` (built-in) |
| [F](#f-data-validation) | Data Validation | concept | validating JSON messages and YAML configs with JSON Schema | `jsonschema` |
| [G](#g-formats-comparison--which-format-for-which-job) | Formats Comparison | summary | same data in every format, decision guide, cheat sheet | – |

Every format section follows the same structure:

1. What the format is and its specification.
2. Syntax with IoT examples.
3. Where it is typically used.
4. Python examples: reading and writing files, converting strings/bytes to Python objects and back.
5. Common pitfalls.

**Suggested reading order:**

- **JSON:** a good starting point, since it is the format most frequently used for messages and APIs.
- **YAML:** the format usually adopted for configuration files, such as `docker-compose.yml`.
- **TOML:** a common alternative to YAML for application and project settings.
- **CSV:** a simple option for storing telemetry logs and datasets for analysis.
- **Data Validation:** how to make components more robust against wrong or unexpected data.
- **Formats Comparison:** a quick reference to compare the formats and choose among them.

---

## A.3 Python Requirements

The Python examples use the standard library (`json`, `csv`, `tomllib`) and the following external packages:

```text
pyyaml>=6.0
jsonschema>=4.18
tomli-w>=1.0
```

They can be installed with:

```bash
pip install pyyaml jsonschema tomli-w
```

---

# B. JSON – JavaScript Object Notation

This section introduces JSON and Python's built-in `json` module using examples from the IoT world
(sensors, actuators, gateways and smart homes).

---

## B.1 What is JSON

JSON (JavaScript Object Notation) is a lightweight, text-based data-interchange format that is easy for humans to read
and write, and easy for machines to parse and generate. It is language-independent, which makes it well suited for
exchanging data between different systems.

- Specification: [RFC 8259](https://www.rfc-editor.org/rfc/rfc8259) and [json.org](https://www.json.org/)
- Media type: `application/json`
- File extension: `.json`

This is one of the reasons why JSON is widely used in IoT: a temperature sensor, a gateway, a cloud platform and a mobile
app can be written in different languages and run on very different hardware, but they can usually all read and write
the same JSON messages.

An example of a JSON document describing a single temperature measurement:

```json
{
  "device_id": "temp-sensor-01",
  "value": 21.5,
  "unit": "C"
}
```

---

## B.2 Basic components of JSON

**Objects**
- Enclosed in curly braces (`{}`).
- Contain key-value pairs separated by commas.
- Keys are strings (always in double quotes).
- Values can be any valid JSON data type (objects, arrays, strings, numbers, booleans, or null).

**Arrays**
- Enclosed in square brackets (`[]`).
- Contain ordered lists of values separated by commas.
- Values can be any valid JSON data type.

**Data types** (with IoT examples):

| JSON type | Example | Python type |
|-----------|---------|-------------|
| String    | `"device_id": "smart-light-03"` | `str` |
| Number    | `"timestamp": 1727535600000`, `"value": 48.7` | `int` / `float` |
| Boolean   | `"online": true` | `bool` (`True` / `False`) |
| Null      | `"value": null` (no measurement yet) | `None` |
| Object    | `"location": {"room": "kitchen"}` | `dict` |
| Array     | `"supported_units": ["C", "F", "K"]` | `list` |

**Characteristics of JSON**

- Human-readable: a developer can usually open a device message and understand it quickly.
- Machine-readable: devices, gateways and cloud services can easily parse and generate it.
- Lightweight: generally more compact than XML, which can be relevant for constrained devices and networks.
- Language-independent: firmware in C, a gateway in Python and a dashboard in JavaScript can share the same data.
- Hierarchical: data is organized using nested objects and arrays (e.g., a home containing rooms containing devices).

Example of a richer JSON structure describing a device, combining all the data types, nested objects and arrays:

```json
{
  "device_id": "temp-sensor-01",
  "device_type": "TemperatureSensor",
  "device_manufacturer": "Acme Inc.",
  "online": true,
  "firmware_version": "1.4.2",
  "location": {
    "room": "kitchen",
    "latitude": 44.6471,
    "longitude": 10.9252
  },
  "supported_units": ["C", "F", "K"],
  "last_measurement": {
    "value": 21.5,
    "unit": "C",
    "timestamp": 1727535600000
  },
  "error": null
}
```

---

## B.3 Where JSON is used in IoT

JSON is often the default choice for data that **moves** between components. Some typical use cases:

### B.3.1 Data structures (device descriptions, digital twins)

A gateway or a cloud platform commonly keeps a description of the connected devices: the example above shows a
possible device descriptor. The same structure can be stored in a file, a document database (e.g., MongoDB) or returned by an API.

### B.3.2 Messages (commands and events)

Devices and applications exchange messages through protocols like MQTT or CoAP. The payload is often a small JSON object.

A **command** sent to an actuator on topic `home/kitchen/light-03/command`:

```json
{"command_id": "c-8812", "action": "SWITCH", "payload": "ON", "timestamp": 1727535600000}
```

An **event** published by a device on topic `home/kitchen/door-01/event`:

```json
{"device_id": "door-01", "event": "DOOR_OPENED", "timestamp": 1727535612000}
```

### B.3.3 Telemetry (measurements)

Sensors commonly publish measurements periodically. To reduce overhead, multiple samples can be grouped in a single batch:

```json
{
  "device_id": "temp-sensor-01",
  "unit": "C",
  "samples": [
    {"t": 1727535600000, "v": 21.5},
    {"t": 1727535601000, "v": 21.7},
    {"t": 1727535602000, "v": 21.4}
  ]
}
```

> Projects often define their own telemetry structure. SenML is a standard JSON (and CBOR)
> representation for sensor measurements that can help address this interoperability problem.

### B.3.4 REST / HTTP APIs

JSON is the most commonly used format for HTTP APIs. Typically, the client sends a request body and declares its format with the
`Content-Type` header; the server replies with a JSON body.

```http
POST /api/v1/devices HTTP/1.1
Host: iot.example.com
Content-Type: application/json

{"device_id": "hum-sensor-02", "device_type": "HumiditySensor", "room": "bathroom"}
```

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/v1/devices/hum-sensor-02

{"device_id": "hum-sensor-02", "device_type": "HumiditySensor", "room": "bathroom", "created_at": "2024-09-28T15:00:00Z"}
```

---

## B.4 Python: the json module

Python provides built-in support for JSON through the `json` module (no installation required):

```python
import json
```

The module has four main functions. The ones ending with **s** work with **s**trings, the others with files:

| Function | From | To |
|----------|------|----|
| `json.load(f)`   | JSON file   | Python object |
| `json.dump(obj, f)` | Python object | JSON file |
| `json.loads(s)`  | JSON string | Python object |
| `json.dumps(obj)` | Python object | JSON string |

---

## B.5 Load JSON data from a file

Given a `devices_config.json` file:

```json
{
  "home_id": "SH001",
  "devices": [
    {"device_id": "temp-sensor-01", "device_type": "TemperatureSensor", "sampling_period_ms": 1000},
    {"device_id": "hum-sensor-02", "device_type": "HumiditySensor", "sampling_period_ms": 5000},
    {"device_id": "smart-light-03", "device_type": "SmartLight"}
  ]
}
```

`json.load()` reads the file and converts it into Python objects (dictionaries and lists):

```python
import json

with open("devices_config.json", "r", encoding="utf-8") as f:
    config = json.load(f)

print(config["home_id"])                     # Output: SH001
for device in config["devices"]:
    # .get() returns a default value when an optional key is missing
    print(device["device_id"], device.get("sampling_period_ms", "n/a"))
```

Output:

```text
SH001
temp-sensor-01 1000
hum-sensor-02 5000
smart-light-03 n/a
```

---

## B.6 Dump data to a JSON file

`json.dump()` writes Python objects to a file as JSON, for example to save the measurements collected by a sensor:

```python
import json

measurements = [
    {"device_id": "temp-sensor-01", "value": 21.5, "unit": "C", "timestamp": 1727535600000},
    {"device_id": "temp-sensor-01", "value": 21.7, "unit": "C", "timestamp": 1727535601000},
    {"device_id": "temp-sensor-01", "value": 21.4, "unit": "C", "timestamp": 1727535602000}
]

with open("temperature_log.json", "w", encoding="utf-8") as f:
    json.dump(measurements, f, indent=2)
```

The optional `indent` parameter produces a nicely formatted (human-readable) file.

---

## B.7 JSON strings and dictionaries

`json.dumps()` and `json.loads()` work with strings instead of files. This is typically what happens when a device sends a message
over the network: data is converted into a JSON string (and then into bytes) before being transmitted, and converted
back into a dictionary by the receiver.

From dictionary to JSON string (e.g., a sensor preparing a message to publish):

```python
import json

measurement = {
    "device_id": "hum-sensor-02",
    "value": 48.7,
    "unit": "%RH",
    "timestamp": 1727535600000
}

json_message = json.dumps(measurement)
print(json_message)
# Output: {"device_id": "hum-sensor-02", "value": 48.7, "unit": "%RH", "timestamp": 1727535600000}

payload = json_message.encode("utf-8")   # bytes, ready to be sent with MQTT/CoAP/HTTP
```

From JSON string to dictionary (e.g., a smart light receiving a command):

```python
import json

json_command = '{"device_id": "smart-light-03", "action": "SWITCH", "payload": "ON"}'
command = json.loads(json_command)

print(command["device_id"])     # Output: smart-light-03
print(command["action"])        # Output: SWITCH
print(command["payload"])       # Output: ON
```

> `json.loads()` also accepts `bytes` directly, so a received MQTT payload (`msg.payload`) can be passed as it is.

---

## B.8 Formatting options

```python
import json

data = {"value": 21.5, "device_id": "temp-sensor-01", "unit": "°C"}

print(json.dumps(data, indent=2))                    # pretty print (files, logs, debugging)
print(json.dumps(data, sort_keys=True))              # stable key order (diffs, tests, hashing)
print(json.dumps(data, separators=(",", ":")))       # compact: no spaces (smaller network payloads)
print(json.dumps(data, ensure_ascii=False))          # keep non-ASCII chars (°) instead of \u00b0
```

Output:

```text
{
  "value": 21.5,
  "device_id": "temp-sensor-01",
  "unit": "\u00b0C"
}
{"device_id": "temp-sensor-01", "unit": "\u00b0C", "value": 21.5}
{"value":21.5,"device_id":"temp-sensor-01","unit":"\u00b0C"}
{"value": 21.5, "device_id": "temp-sensor-01", "unit": "°C"}
```

---

## B.9 Serializing custom objects and dates

`json.dumps()` only knows the basic Python types (`dict`, `list`, `str`, `int`, `float`, `bool`, `None`).
Objects of your classes and `datetime` values raise a `TypeError`. There are two common solutions.

**1. Convert the object to a dictionary explicitly** (usually a clear option for your own classes):

```python
import json

class Measurement:
    def __init__(self, device_id, value, unit, timestamp):
        self.device_id = device_id
        self.value = value
        self.unit = unit
        self.timestamp = timestamp

    def to_dict(self):
        return {"device_id": self.device_id, "value": self.value,
                "unit": self.unit, "timestamp": self.timestamp}

    @classmethod
    def from_dict(cls, data):
        return cls(data["device_id"], data["value"], data["unit"], data["timestamp"])

m = Measurement("temp-sensor-01", 21.5, "C", 1727535600000)
message = json.dumps(m.to_dict())              # object -> JSON
received = Measurement.from_dict(json.loads(message))   # JSON -> object
print(received.value)                          # Output: 21.5
```

> With dataclasses you can use `dataclasses.asdict(obj)` instead of writing `to_dict()` by hand.

**2. Use the `default` parameter** to tell `json` how to convert unknown types (handy for `datetime`):

```python
import json
from datetime import datetime, timezone

def json_default(obj):
    if isinstance(obj, datetime):
        return obj.isoformat()          # ISO 8601 string, e.g. 2024-09-28T15:00:00+00:00
    raise TypeError(f"Type {type(obj).__name__} is not JSON serializable")

event = {"device_id": "door-01", "event": "DOOR_OPENED",
         "time": datetime(2024, 9, 28, 15, 0, 0, tzinfo=timezone.utc)}

print(json.dumps(event, default=json_default))
# Output: {"device_id": "door-01", "event": "DOOR_OPENED", "time": "2024-09-28T15:00:00+00:00"}
```

> JSON has no date type: timestamps are usually represented either as **Unix epoch numbers** (seconds or milliseconds)
> or as **ISO 8601 strings**. It is good practice to choose one convention and document it.

---

## B.10 Handling invalid JSON

Data coming from the network may be malformed, so it is good practice to catch `json.JSONDecodeError`:

```python
import json

raw_payload = b'{"device_id": "temp-sensor-01", "value": 21.5,}'   # trailing comma -> invalid

try:
    data = json.loads(raw_payload)
except json.JSONDecodeError as e:
    print(f"Invalid JSON message: {e.msg} (line {e.lineno}, column {e.colno})")
    # Output: Invalid JSON message: Expecting property name enclosed in double quotes (line 1, column 47)
```

A syntactically valid JSON can still have missing fields or wrong types: see [Data Validation](#f-data-validation).

---

## B.11 Common pitfalls

- **No comments** are allowed in JSON (YAML or TOML are commonly used for configuration files that need comments).
- **No trailing commas** after the last element of an object or array.
- **Keys and strings require double quotes**: `'device'` (single quotes) is invalid.
- `True`/`False`/`None` are Python; in JSON they are `true`/`false`/`null`.
- **Dictionary keys become strings**: `json.dumps({1: "a"})` → `{"1": "a"}`.
- **Tuples become arrays** and come back as lists.
- **Floats**: `NaN` and `Infinity` are produced by Python but are not valid standard JSON (use `allow_nan=False` to
  detect them); floating-point precision may differ between languages.
- Opening files with an explicit `encoding="utf-8"` is recommended, to avoid platform-dependent defaults.

---

# C. YAML – YAML Ain't Markup Language

This section introduces YAML, one of the formats most commonly used for **configuration files**, and shows how to read and write it
in Python with the PyYAML library.

---

## C.1 What is YAML

YAML is a human-friendly data serialization format. It represents the same data model as JSON (mappings, sequences and
scalar values) but uses **indentation** instead of braces and brackets, and supports **comments**.
These features make it a frequent choice for files written and maintained **by humans**, such as configurations.

- Specification: [yaml.org](https://yaml.org/) (current version 1.2)
- Media type: `application/yaml`
- File extensions: `.yaml` or `.yml`

The same data in JSON and YAML:

```json
{
  "device_id": "temp-sensor-01",
  "sampling_period_ms": 1000,
  "units": ["C", "F"],
  "mqtt": {"host": "localhost", "port": 1883}
}
```

```yaml
# Temperature sensor configuration
device_id: temp-sensor-01
sampling_period_ms: 1000
units:
  - C
  - F
mqtt:
  host: localhost
  port: 1883
```

> YAML 1.2 is (almost) a **superset of JSON**: a valid JSON document is also a valid YAML document.

---

## C.2 Basic syntax

**Mappings** (key-value pairs, like JSON objects / Python dictionaries) use `key: value`.
Nesting is expressed with indentation (**spaces only, tabs are not allowed**; 2 spaces is the usual convention):

```yaml
gateway:
  id: gw-01
  location:
    building: A
    floor: 2
```

**Sequences** (lists, like JSON arrays) use a dash `-` followed by a space:

```yaml
sensors:
  - temp-sensor-01
  - hum-sensor-02
```

Sequences of mappings are quite common (e.g., a list of devices):

```yaml
devices:
  - device_id: temp-sensor-01
    type: TemperatureSensor
    sampling_period_ms: 1000
  - device_id: smart-light-03
    type: SmartLight
```

**Scalars** are typed automatically:

```yaml
name: temp-sensor-01      # string (quotes are optional)
description: "Kitchen sensor: main"   # quotes needed when the value contains ": " or starts with special chars
port: 1883                # int
threshold: 25.5           # float
enabled: true             # bool
last_value: null          # null (also ~)
```

**Comments** start with `#` and are ignored by the parser.

**Flow style**: JSON-like inline syntax is also allowed, useful for short lists or mappings:

```yaml
units: [C, F, K]
position: {lat: 44.6471, lon: 10.9252}
```

---

## C.3 Advanced features

**Multi-line strings**: `|` (literal) keeps newlines, `>` (folded) joins lines with spaces:

```yaml
certificate: |
  -----BEGIN CERTIFICATE-----
  MIIBszCCAVmgAwIBAgIU...
  -----END CERTIFICATE-----
description: >
  This gateway collects data from all the sensors
  of the second floor and forwards it to the cloud.
```

**Anchors (`&`) and aliases (`*`)** help avoid repetition. With the merge key `<<` a mapping can inherit default values:

```yaml
defaults: &sensor_defaults
  sampling_period_ms: 1000
  unit: C
  enabled: true

sensors:
  - device_id: temp-sensor-01
    <<: *sensor_defaults          # inherits all default values
  - device_id: temp-sensor-02
    <<: *sensor_defaults
    sampling_period_ms: 5000      # overrides a single value
```

**Multiple documents** in the same file are separated by `---` (widely used, for example, in Kubernetes manifests).

---

## C.4 Where YAML is used in IoT

YAML is commonly used for data that **configures** systems and is edited by people, for example:

- **Device and gateway configuration**: broker address, topics, sampling periods, thresholds, list of sensors.
- **Container orchestration**: `docker-compose.yml` and Kubernetes manifests.
- **Edge/IoT platforms**: Home Assistant, ESPHome, Eclipse Ditto/Hono deployments, Node-RED settings.
- **CI/CD pipelines**: GitHub Actions workflows (`.github/workflows/*.yml`), GitLab CI.
- **API specifications**: OpenAPI documents are typically written in YAML.

Example: a `docker-compose.yml` for an IoT stack:

```yaml
services:
  mqtt-broker:
    image: eclipse-mosquitto:2.0
    ports:
      - "1883:1883"
  http-api:
    build: ./http-api
    environment:
      - MQTT_BROKER_HOST=mqtt-broker
    depends_on:
      - mqtt-broker
```

> As a general rule of thumb, YAML is usually preferred for **configuration written by humans**, while JSON is more
> common for **messages exchanged by machines**. Exceptions exist in both directions.

---

## C.5 Python: the PyYAML library

YAML is not part of the Python standard library. Install [PyYAML](https://pyyaml.org/):

```bash
pip install pyyaml
```

```python
import yaml
```

| Function | From | To |
|----------|------|----|
| `yaml.safe_load(stream)` | YAML string or file | Python object |
| `yaml.safe_dump(obj, stream)` | Python object | YAML file (or string if `stream` is omitted) |
| `yaml.safe_load_all(stream)` | multi-document YAML | iterator of Python objects |
| `yaml.safe_dump_all(objs, stream)` | list of Python objects | multi-document YAML |

> ⚠️ **Using `safe_load` is strongly recommended**. The generic `yaml.load()` with the unsafe loader can build arbitrary Python
> objects and execute code if the file comes from an untrusted source. `safe_load` only creates basic types
> (dict, list, str, int, float, bool, None, dates).

---

## C.6 Load a YAML configuration file

Given a `gateway_config.yaml` file:

```yaml
# Gateway configuration
gateway_id: gw-01
mqtt:
  host: localhost
  port: 1883
  base_topic: home/sensors
devices:
  - device_id: temp-sensor-01
    type: TemperatureSensor
    sampling_period_ms: 1000
  - device_id: hum-sensor-02
    type: HumiditySensor
    sampling_period_ms: 5000
```

```python
import yaml

with open("gateway_config.yaml", "r", encoding="utf-8") as f:
    config = yaml.safe_load(f)

print(type(config))                       # Output: <class 'dict'>
print(config["mqtt"]["host"], config["mqtt"]["port"])   # Output: localhost 1883

for device in config["devices"]:
    topic = f"{config['mqtt']['base_topic']}/{device['device_id']}"
    print(device["device_id"], device["sampling_period_ms"], topic)
```

Output:

```text
<class 'dict'>
localhost 1883
temp-sensor-01 1000 home/sensors/temp-sensor-01
hum-sensor-02 5000 home/sensors/hum-sensor-02
```

Once loaded, the configuration is a normal Python dictionary: essentially the same kind of structure you would get
from `json.load()`.

---

## C.7 Write a YAML file

```python
import yaml

config = {
    "gateway_id": "gw-02",
    "mqtt": {"host": "broker.example.com", "port": 8883, "tls": True},
    "devices": [
        {"device_id": "temp-sensor-05", "type": "TemperatureSensor", "sampling_period_ms": 2000},
    ],
}

with open("new_gateway_config.yaml", "w", encoding="utf-8") as f:
    yaml.safe_dump(config, f, sort_keys=False, default_flow_style=False)

print(yaml.safe_dump(config, sort_keys=False))   # without a stream it returns a string
```

Output:

```yaml
gateway_id: gw-02
mqtt:
  host: broker.example.com
  port: 8883
  tls: true
devices:
- device_id: temp-sensor-05
  type: TemperatureSensor
  sampling_period_ms: 2000
```

- `sort_keys=False` keeps the original key order (by default keys are sorted alphabetically).
- `default_flow_style=False` forces the block (indented) style.
- `allow_unicode=True` writes non-ASCII characters as they are.
- Comments are **not** preserved: `safe_load` discards them and `safe_dump` cannot write them
  (the [`ruamel.yaml`](https://pypi.org/project/ruamel.yaml/) library supports round-trip editing with comments).

---

## C.8 Multiple documents in one file

```python
import yaml

text = """
device_id: temp-sensor-01
type: TemperatureSensor
---
device_id: smart-light-03
type: SmartLight
"""

for doc in yaml.safe_load_all(text):
    print(doc)
```

Output:

```text
{'device_id': 'temp-sensor-01', 'type': 'TemperatureSensor'}
{'device_id': 'smart-light-03', 'type': 'SmartLight'}
```

---

## C.9 Converting between YAML and JSON

Since both formats share (almost) the same data model, converting is usually just "load with one, dump with the
other". A possible scenario: the configuration is written in YAML by an operator, and sent as JSON to a device through MQTT
or an HTTP API.

```python
import json
import yaml

with open("gateway_config.yaml", "r", encoding="utf-8") as f:
    config = yaml.safe_load(f)

json_config = json.dumps(config)              # YAML -> JSON (e.g., to send it over the network)
back_to_yaml = yaml.safe_dump(json.loads(json_config), sort_keys=False)   # JSON -> YAML
print(json_config)
```

---

## C.10 Common pitfalls

- **Tabs are not allowed** for indentation, and wrong indentation can silently change the structure.
- **Implicit typing surprises** (YAML 1.1, used by PyYAML):
  - `country: NO` → `False` (the "Norway problem"); also `yes/no/on/off` are booleans.
  - `version: 1.10` → float `1.1`; `zip: 01234` → may be parsed as an octal integer.
  - `time: 12:30` → may become a number (sexagesimal) in YAML 1.1.
  - **A common solution** is to quote values that need to remain strings: `version: "1.10"`, `country: "NO"`.
- `key:value` (without space) is a string, not a mapping: a space after `:` is needed.
- Values containing `: ` or starting with `{`, `[`, `*`, `&`, `#`, `@` usually need to be quoted.
- An empty file is loaded as `None`, not as an empty dictionary: it is worth handling this case (e.g., `config = yaml.safe_load(f) or {}`).
- A configuration can be syntactically valid and still wrong: see [Data Validation](#f-data-validation).

```python
import yaml

print(yaml.safe_load("country: NO\nversion: 1.10\nenabled: on"))
# Output: {'country': False, 'version': 1.1, 'enabled': True}
print(yaml.safe_load('country: "NO"\nversion: "1.10"\nenabled: "on"'))
# Output: {'country': 'NO', 'version': '1.10', 'enabled': 'on'}
```

---

# D. TOML – Tom's Obvious, Minimal Language

This section introduces TOML, a configuration format designed to be simple and unambiguous, and shows how to read it
with Python's built-in `tomllib` module (and write it with `tomli-w`).

---

## D.1 What is TOML

TOML is a configuration file format designed to map unambiguously to a dictionary (hash table). It looks similar to
the classic `.ini` files, but with a precise specification and **explicit types**.

- Specification: [toml.io](https://toml.io/en/v1.0.0) (version 1.0.0)
- Media type: `application/toml`
- File extension: `.toml`

```toml
# Gateway configuration
gateway_id = "gw-01"
log_level = "INFO"

[mqtt]
host = "localhost"
port = 1883
tls = false
```

---

## D.2 Syntax

**Key/value pairs**: `key = value`. Strings are quoted, which helps avoid ambiguity about types:

```toml
name = "temp-sensor-01"     # string
port = 1883                 # integer
threshold = 25.5            # float
enabled = true              # boolean
version = "1.10"            # string (quoted, so it stays a string)
installed = 2024-09-28T15:00:00Z   # date-time (native type!)
units = ["Cel", "K"]        # array
position = { lat = 44.6471, lon = 10.9252 }   # inline table
```

TOML has **no null** value: a missing key is usually interpreted as "not set".

**Tables** (sections) group keys, like nested dictionaries. Dotted names create nested tables:

```toml
[mqtt]
host = "localhost"
port = 1883

[mqtt.auth]          # nested table: config["mqtt"]["auth"]
username = "gateway"
password_env = "MQTT_PASSWORD"
```

**Arrays of tables** (`[[...]]`) define lists of objects, e.g. a list of devices:

```toml
[[devices]]
device_id = "temp-sensor-01"
type = "TemperatureSensor"
sampling_period_ms = 1000

[[devices]]
device_id = "hum-sensor-02"
type = "HumiditySensor"
sampling_period_ms = 5000
```

---

## D.3 Where TOML is used

Some common examples:

- **Python projects**: `pyproject.toml` (project metadata, dependencies, tool settings for pytest, ruff, black...).
- **Rust projects**: `Cargo.toml`.
- **Application configuration**: Telegraf (the InfluxData agent, often used for IoT metrics collection),
  InfluxDB, Hugo, Gitea, Poetry, uv...

Example: a fragment of a Telegraf configuration that subscribes to MQTT telemetry:

```toml
[[inputs.mqtt_consumer]]
servers = ["tcp://localhost:1883"]
topics = ["home/sensors/#"]
data_format = "json"
```

---

## D.4 Python: read TOML with tomllib

Since Python 3.11, the standard library includes `tomllib` (read-only). Note that the file must be opened in
**binary mode** (`"rb"`).

Given a `gateway_config.toml` file:

```toml
gateway_id = "gw-01"
installed = 2024-09-28T15:00:00Z

[mqtt]
host = "localhost"
port = 1883
base_topic = "home/sensors"

[[devices]]
device_id = "temp-sensor-01"
type = "TemperatureSensor"
sampling_period_ms = 1000

[[devices]]
device_id = "hum-sensor-02"
type = "HumiditySensor"
sampling_period_ms = 5000
```

```python
import tomllib

with open("gateway_config.toml", "rb") as f:
    config = tomllib.load(f)

print(config["mqtt"]["host"], config["mqtt"]["port"])   # Output: localhost 1883
print(repr(config["installed"]))
# Output: datetime.datetime(2024, 9, 28, 15, 0, tzinfo=datetime.timezone.utc)

for device in config["devices"]:
    print(device["device_id"], device["sampling_period_ms"])
```

Output:

```text
localhost 1883
datetime.datetime(2024, 9, 28, 15, 0, tzinfo=datetime.timezone.utc)
temp-sensor-01 1000
hum-sensor-02 5000
```

From a string, use `tomllib.loads()`:

```python
import tomllib

config = tomllib.loads('log_level = "DEBUG"\n[mqtt]\nport = 8883')
print(config)   # Output: {'log_level': 'DEBUG', 'mqtt': {'port': 8883}}
```

> For Python < 3.11 install the `tomli` package: it has the same API (`import tomli as tomllib`).

---

## D.5 Python: write TOML with tomli-w

`tomllib` can only read. To write TOML files install `tomli-w`:

```bash
pip install tomli-w
```

```python
import tomli_w

config = {
    "gateway_id": "gw-02",
    "mqtt": {"host": "broker.example.com", "port": 8883, "tls": True},
    "devices": [
        {"device_id": "temp-sensor-05", "type": "TemperatureSensor", "sampling_period_ms": 2000},
    ],
}

with open("new_gateway_config.toml", "wb") as f:   # binary mode here too
    tomli_w.dump(config, f)

print(tomli_w.dumps(config))
```

Output:

```toml
gateway_id = "gw-02"
devices = [
    { device_id = "temp-sensor-05", type = "TemperatureSensor", sampling_period_ms = 2000 },
]

[mqtt]
host = "broker.example.com"
port = 8883
tls = true
```

`tomli-w` writes the list of devices as an array of **inline tables**: it is equivalent to the `[[devices]]` syntax
shown above (`tomllib` loads both into the same Python list of dictionaries).

---

## D.6 TOML vs YAML for configuration

| | TOML | YAML |
|---|------|------|
| Readability | Usually very good for flat or 2-level configs | Usually very good, also for deep nesting |
| Typing | Explicit (strings are quoted) | Implicit (possible surprises like `NO` → `false`) |
| Deep nesting | Can become verbose (`[a.b.c]`) | Natural (indentation) |
| Comments | ✅ | ✅ |
| Dates | Native type | Supported (implicit) |
| Null | ❌ | ✅ |
| Python support | Read: built-in (`tomllib`); write: `tomli-w` | `PyYAML` (external) |
| Common uses | `pyproject.toml`, app settings | docker-compose, Kubernetes, CI pipelines |

As a general rule of thumb, TOML is often a good fit for **application settings** that are mostly key/value, while
YAML is usually preferred for **deeply nested** configurations or when the ecosystem already expects it
(Docker, Kubernetes, GitHub Actions...). The choice also depends on team habits and tooling.

---

## D.7 Common pitfalls

- `tomllib` requires files opened in **binary mode** (`"rb"`); `tomli_w.dump` requires `"wb"`.
- `tomllib` is read-only: a separate library such as `tomli-w` is needed to write. Comments are typically lost when writing (like YAML with PyYAML).
- There is no `null`: `tomli_w` cannot serialize `None` values, so those keys are usually removed before writing.
- A table can be defined only once: repeating `[mqtt]` twice in the same file is an error.
- Keys after a `[table]` header belong to that table until the next header: it is common practice to put top-level keys **before** any table.

---

# E. CSV – Comma-Separated Values

This section introduces CSV, one of the simplest formats for **tabular data** such as telemetry logs and datasets,
and shows how to read and write it with Python's built-in `csv` module.

---

## E.1 What is CSV

CSV is a plain text format where each **line is a row** and **values are separated by commas**.
The first row usually contains the column names (header).

- Specification: [RFC 4180](https://www.rfc-editor.org/rfc/rfc4180) (informal, many dialects exist)
- Media type: `text/csv`
- File extension: `.csv`

```csv
device_id,timestamp,value,unit
temp-sensor-01,1727535600,21.5,Cel
temp-sensor-01,1727535610,21.7,Cel
hum-sensor-02,1727535600,48.7,%RH
```

CSV is **flat** (no nesting) and **untyped** (everything is text), but it is usually compact for tables, can be
appended line by line, and can be opened by many tools: Excel, LibreOffice, pandas, R, databases, Grafana...

---

## E.2 Syntax

- One record per line; fields separated by a delimiter (`,` by default; `;` or `\t` are common alternatives).
- Fields containing the delimiter, double quotes or newlines are enclosed in double quotes.
- A double quote inside a quoted field is escaped by doubling it: `""`.

```csv
device_id,location,note
temp-sensor-01,"Building A, floor 2","calibrated ""manually"""
```

---

## E.3 Where CSV is used in IoT

Some typical use cases:

- **Telemetry logs** on a gateway or an edge device (append one row per measurement).
- **Datasets** for data analysis and machine learning (export from a time-series database).
- **Bulk import/export** of device inventories (list of devices, serial numbers, locations).
- **Data exchange with non-developers** (spreadsheets).

CSV is usually **not** a good fit for messages between services (no structure, no types, no nesting): JSON or CBOR
are generally more suitable in that case.

---

## E.4 Python: the csv module

The `csv` module is built-in:

```python
import csv
```

| Class / function | Use |
|------------------|-----|
| `csv.writer(f)` / `csv.reader(f)` | rows as **lists** |
| `csv.DictWriter(f, fieldnames)` / `csv.DictReader(f)` | rows as **dictionaries** (column name → value) |

> Opening CSV files with `newline=""` is recommended: the `csv` module handles line endings by itself; without it you may get
> empty lines between rows on Windows.

---

## E.5 Write a CSV file

With `csv.writer` (rows as lists):

```python
import csv

rows = [
    ["temp-sensor-01", 1727535600, 21.5, "Cel"],
    ["temp-sensor-01", 1727535610, 21.7, "Cel"],
    ["hum-sensor-02", 1727535600, 48.7, "%RH"],
]

with open("telemetry.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    writer.writerow(["device_id", "timestamp", "value", "unit"])   # header
    writer.writerows(rows)
```

With `csv.DictWriter` (rows as dictionaries, often a convenient choice when data comes from JSON messages):

```python
import csv

measurements = [
    {"device_id": "temp-sensor-01", "timestamp": 1727535600, "value": 21.5, "unit": "Cel"},
    {"device_id": "hum-sensor-02", "timestamp": 1727535600, "value": 48.7, "unit": "%RH"},
]

with open("telemetry.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=["device_id", "timestamp", "value", "unit"])
    writer.writeheader()
    writer.writerows(measurements)
```

---

## E.6 Read a CSV file

```python
import csv

with open("telemetry.csv", "r", newline="", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row)
```

Output:

```text
{'device_id': 'temp-sensor-01', 'timestamp': '1727535600', 'value': '21.5', 'unit': 'Cel'}
{'device_id': 'hum-sensor-02', 'timestamp': '1727535600', 'value': '48.7', 'unit': '%RH'}
```

⚠️ All values are **strings**, so they usually need to be converted explicitly:

```python
import csv

with open("telemetry.csv", "r", newline="", encoding="utf-8") as f:
    measurements = [
        {**row, "timestamp": int(row["timestamp"]), "value": float(row["value"])}
        for row in csv.DictReader(f)
    ]

temperatures = [m["value"] for m in measurements if m["unit"] == "Cel"]
print("Average temperature:", sum(temperatures) / len(temperatures))   # Output: Average temperature: 21.5
```

---

## E.7 Append measurements to a log

A gateway can store every received measurement by appending a row (`"a"` mode), writing the header only when the file
is new:

```python
import csv
import os

LOG_FILE = "telemetry_log.csv"
FIELDS = ["device_id", "timestamp", "value", "unit"]

def log_measurement(measurement):
    is_new_file = not os.path.exists(LOG_FILE)
    with open(LOG_FILE, "a", newline="", encoding="utf-8") as f:
        writer = csv.DictWriter(f, fieldnames=FIELDS, extrasaction="ignore")   # ignore extra keys
        if is_new_file:
            writer.writeheader()
        writer.writerow(measurement)

log_measurement({"device_id": "temp-sensor-01", "timestamp": 1727535600, "value": 21.5, "unit": "Cel"})
log_measurement({"device_id": "temp-sensor-01", "timestamp": 1727535610, "value": 21.7, "unit": "Cel", "rssi": -70})
```

---

## E.8 From JSON messages to CSV

Nested JSON typically needs to be **flattened** before writing it to CSV. For example, a SenML pack, once resolved,
becomes one row per record:

```python
import csv
import json

payload = '''[
  {"n": "urn:dev:mac:0024befffe804ff1:temperature", "u": "Cel", "v": 21.5, "t": 1727535600},
  {"n": "urn:dev:mac:0024befffe804ff1:humidity", "u": "%RH", "v": 48.7, "t": 1727535600}
]'''

with open("senml_log.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=["n", "t", "v", "u"])
    writer.writeheader()
    writer.writerows(json.loads(payload))
```

Result (`senml_log.csv`):

```csv
n,t,v,u
urn:dev:mac:0024befffe804ff1:temperature,1727535600,21.5,Cel
urn:dev:mac:0024befffe804ff1:humidity,1727535600,48.7,%RH
```

---

## E.9 Analysis with pandas (optional)

For data analysis, [pandas](https://pandas.pydata.org/) reads a CSV directly into a table (DataFrame) and infers types:

```python
import pandas as pd   # pip install pandas

df = pd.read_csv("telemetry.csv")
print(df.groupby("device_id")["value"].mean())
```

---

## E.10 Common pitfalls

- **No types**: everything is a string, numbers and booleans need to be converted manually.
- **No nesting**: nested objects/arrays need to be flattened (e.g. `location.room` → `location_room`).
- **Dialects**: delimiter (`,` vs `;`), quoting and decimal separator vary. Italian Excel, for example, uses `;` as
  delimiter and `,` as decimal separator (`21,5`). `csv.reader(f, delimiter=";")` can be used in these cases.
- Opening files with `newline=""` and an explicit `encoding="utf-8"` is recommended.
- Keeping the column order fixed is usually a good idea (e.g., `DictWriter` with an explicit `fieldnames` list).

---

# F. Data Validation

This section introduces the concept of **data validation**, explains why it is particularly important in distributed IoT systems,
and shows how to validate JSON messages and YAML configuration files in Python using **JSON Schema**.

---

## F.1 Why validate data

In a distributed system, data often comes from **components you do not fully control**: devices with different firmware versions,
third-party services, users editing configuration files. A JSON message or a YAML file can be **perfectly valid
syntactically** and still be wrong:

```json
{"device_id": "temp-sensor-01", "value": "hot", "unit": "C"}
```

```yaml
mqtt:
  host: localhost
  port: "eighteen-eighty-three"
```

Without validation, the error may show up much later and far from its cause, for example a `TypeError` in the analytics service,
a crash when connecting to the broker, a wrong value stored in the database or shown in a dashboard.

Validation means **checking that data respects an agreed contract before using it**. Some benefits:

- **Fail fast**: errors can be detected at the boundary, with a clear message saying what is wrong and where.
- **Robustness**: a malformed message from one device is less likely to crash the whole gateway.
- **Security**: untrusted input can be rejected before reaching the application logic (e.g., huge strings, unexpected fields).
- **Documentation and contracts**: a schema is a precise, machine-readable description of the data exchanged between
  teams and services (producers and consumers can evolve independently as long as they respect it).

---

## F.2 Three levels of validation

| Level | Question | Example error | Tool |
|-------|----------|---------------|------|
| **Syntactic** | Is it well-formed JSON/YAML? | missing quote, trailing comma, wrong indentation | the parser (`json.loads`, `yaml.safe_load`) |
| **Structural (schema)** | Does it have the expected fields with the right types and ranges? | missing `device_id`, `value` is a string, `port` > 65535 | **JSON Schema** |
| **Semantic (business rules)** | Does it make sense in our domain? | unknown device, timestamp in the future, `min_threshold` > `max_threshold` | application code |

The first level is handled by the parsers seen in the [JSON](#b10-handling-invalid-json) and
[YAML](#c-yaml--yaml-aint-markup-language) sections. This section focuses on the second level, which is usually the most reusable one.

---

## F.3 Where to validate

```text
   Device ──MQTT──▶ Gateway ──HTTP──▶ Cloud API ──▶ Database
                       ▲                  ▲
               validate incoming   validate request
                   messages             bodies

  config.yaml ──▶ Application startup
                       ▲
             validate config before
             starting (fail fast)
```

- **Inbound messages** (MQTT/CoAP/HTTP): messages are commonly validated on arrival; on error, a typical approach is
  to log and discard them (or reply with an error, e.g. HTTP `400 Bad Request`) instead of crashing.
- **Configuration files**: usually validated at startup; on error, the application typically stops with a clear message.
- **Outbound messages**: validating them in tests helps ensure that what you produce respects the contract.

---

## F.4 JSON Schema

[JSON Schema](https://json-schema.org/) is a standard vocabulary to describe the structure of JSON data.
A schema is itself a JSON document. Example: the schema of a telemetry message.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "Telemetry message",
  "type": "object",
  "properties": {
    "device_id": {"type": "string", "pattern": "^[a-z0-9-]+$"},
    "value":     {"type": "number"},
    "unit":      {"type": "string", "enum": ["Cel", "%RH", "lx", "W"]},
    "timestamp": {"type": "integer", "minimum": 0}
  },
  "required": ["device_id", "value", "unit", "timestamp"],
  "additionalProperties": false
}
```

Since YAML, TOML and CBOR share (largely) the same data model as JSON (dictionaries, lists, strings, numbers...), **the same
JSON Schema can usually validate data loaded from any of these formats**: validation happens on the Python objects, after
parsing.

---

## F.5 Main JSON Schema keywords

| Keyword | Applies to | Meaning |
|---------|-----------|---------|
| `type` | any | `object`, `array`, `string`, `number`, `integer`, `boolean`, `null` (or a list of types) |
| `enum` / `const` | any | value must be one of a list / exactly a value |
| `properties` | object | schema of each field |
| `required` | object | list of mandatory fields |
| `additionalProperties` | object | `false` rejects unknown fields |
| `minimum` / `maximum` / `exclusiveMinimum` / `exclusiveMaximum` | number | numeric range |
| `minLength` / `maxLength` / `pattern` | string | length and regular expression |
| `format` | string | e.g. `date-time`, `email`, `ipv4`, `uri` (annotation, checked only if enabled) |
| `items` / `minItems` / `maxItems` / `uniqueItems` | array | schema of the elements and size |
| `default` / `description` | any | documentation only (not applied automatically) |
| `$ref` / `$defs` | any | reuse schema fragments |

---

## F.6 Python: the jsonschema library

```bash
pip install jsonschema
```

| Function / class | Use |
|------------------|-----|
| `jsonschema.validate(instance, schema)` | raises `ValidationError` at the first error |
| `Draft202012Validator(schema)` | reusable validator object (faster when validating many messages) |
| `validator.iter_errors(instance)` | iterates over **all** the errors |
| `Draft202012Validator.check_schema(schema)` | checks that the schema itself is valid |

---

## F.7 Validating JSON telemetry messages

```python
import json
from jsonschema import validate, ValidationError

TELEMETRY_SCHEMA = {
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "type": "object",
    "properties": {
        "device_id": {"type": "string", "pattern": "^[a-z0-9-]+$"},
        "value": {"type": "number"},
        "unit": {"type": "string", "enum": ["Cel", "%RH", "lx", "W"]},
        "timestamp": {"type": "integer", "minimum": 0},
    },
    "required": ["device_id", "value", "unit", "timestamp"],
    "additionalProperties": False,
}

def parse_telemetry(payload):
    """Returns the message as a dict, or None if it is not valid."""
    try:
        message = json.loads(payload)                 # 1. syntactic validation
        validate(message, TELEMETRY_SCHEMA)           # 2. structural validation
        return message
    except json.JSONDecodeError as e:
        print(f"Discarded: invalid JSON ({e.msg})")
    except ValidationError as e:
        path = "/".join(str(p) for p in e.absolute_path) or "(root)"
        print(f"Discarded: {path}: {e.message}")
    return None

parse_telemetry('{"device_id": "temp-sensor-01", "value": 21.5, "unit": "Cel", "timestamp": 1727535600}')
parse_telemetry('{"device_id": "temp-sensor-01", "value": "hot", "unit": "Cel", "timestamp": 1727535600}')
parse_telemetry('{"device_id": "temp-sensor-01", "value": 21.5, "unit": "C", "timestamp": 1727535600}')
parse_telemetry('{"device_id": "temp-sensor-01", "value": 21.5, "unit": "Cel"}')
parse_telemetry('{"device_id": "temp-sensor-01", "value": 21.5,')
```

Output (the first message is valid and prints nothing):

```text
Discarded: value: 'hot' is not of type 'number'
Discarded: unit: 'C' is not one of ['Cel', '%RH', 'lx', 'W']
Discarded: (root): 'timestamp' is a required property
Discarded: invalid JSON (Expecting property name enclosed in double quotes)
```

In an MQTT consumer, `parse_telemetry(msg.payload)` would be called inside the `on_message` callback: invalid messages
are logged and discarded while the consumer keeps running.

---

## F.8 Reporting all the errors

`validate()` stops at the first error. When validating a configuration file, it is often more useful to show **all**
the problems at once:

```python
from jsonschema import Draft202012Validator

validator = Draft202012Validator(TELEMETRY_SCHEMA)   # schema from the previous example

message = {"device_id": "Temp Sensor", "value": "hot", "unit": "C", "battery": 87}

for error in sorted(validator.iter_errors(message), key=lambda e: list(e.absolute_path)):
    path = "/".join(str(p) for p in error.absolute_path) or "(root)"
    print(f"- {path}: {error.message}")
```

Output:

```text
- (root): 'timestamp' is a required property
- (root): Additional properties are not allowed ('battery' was unexpected)
- device_id: 'Temp Sensor' does not match '^[a-z0-9-]+$'
- unit: 'C' is not one of ['Cel', '%RH', 'lx', 'W']
- value: 'hot' is not of type 'number'
```

---

## F.9 Validating a YAML configuration

A YAML file is loaded with `yaml.safe_load()` into normal Python dictionaries and lists, which are then validated with a
JSON Schema, in the same way as a JSON message.

Schema of the gateway configuration (nested objects, arrays, ranges and defaults):

```python
GATEWAY_CONFIG_SCHEMA = {
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "type": "object",
    "properties": {
        "gateway_id": {"type": "string", "minLength": 1},
        "mqtt": {
            "type": "object",
            "properties": {
                "host": {"type": "string"},
                "port": {"type": "integer", "minimum": 1, "maximum": 65535},
                "base_topic": {"type": "string"},
            },
            "required": ["host", "port"],
            "additionalProperties": False,
        },
        "devices": {
            "type": "array",
            "minItems": 1,
            "items": {
                "type": "object",
                "properties": {
                    "device_id": {"type": "string"},
                    "type": {"enum": ["TemperatureSensor", "HumiditySensor", "SmartLight"]},
                    "sampling_period_ms": {"type": "integer", "minimum": 100, "default": 1000},
                },
                "required": ["device_id", "type"],
            },
        },
    },
    "required": ["gateway_id", "mqtt", "devices"],
}
```

Loading and validating at startup (fail fast):

```python
import sys
import yaml
from jsonschema import Draft202012Validator

def load_config(path):
    with open(path, "r", encoding="utf-8") as f:
        config = yaml.safe_load(f) or {}          # an empty file returns None

    errors = list(Draft202012Validator(GATEWAY_CONFIG_SCHEMA).iter_errors(config))
    if errors:
        print(f"Invalid configuration file '{path}':")
        for error in errors:
            path_str = "/".join(str(p) for p in error.absolute_path) or "(root)"
            print(f"  - {path_str}: {error.message}")
        sys.exit(1)                               # do not start with a broken configuration
    return config
```

Given this (wrong) `gateway_config.yaml`:

```yaml
gateway_id: gw-01
mqtt:
  host: localhost
  port: 18830000
devices:
  - device_id: temp-sensor-01
    type: TemperatureSensor
    sampling_period_ms: 10
  - device_id: light-01
    type: Light
```

`load_config("gateway_config.yaml")` prints the following and exits:

```text
Invalid configuration file 'gateway_config.yaml':
  - mqtt/port: 18830000 is greater than the maximum of 65535
  - devices/0/sampling_period_ms: 10 is less than the minimum of 100
  - devices/1/type: 'Light' is not one of ['TemperatureSensor', 'HumiditySensor', 'SmartLight']
```

Notice how the error **path** (`devices/1/type`) helps locate the wrong line of the file quickly.

> ⚠️ Schema validation can also catch the YAML implicit typing problems described in the [YAML section](#c10-common-pitfalls):
> if `version: 1.10` is loaded as a float but the schema requires `{"type": "string"}`, the error is reported immediately.

> `default` values are **not** applied by `jsonschema`: a common approach is `device.get("sampling_period_ms", 1000)` in the code
> (or a library like pydantic, see below).

---

## F.10 Writing the schema in YAML

Since a schema is just data, it can be written in YAML too, which many people find more readable and which allows comments.
Keeping the schema in a separate file lets producers and consumers share the same contract:

```yaml
# telemetry.schema.yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: Telemetry message
type: object
properties:
  device_id: {type: string, pattern: "^[a-z0-9-]+$"}
  value: {type: number}
  unit: {type: string, enum: [Cel, "%RH", lx, W]}
  timestamp: {type: integer, minimum: 0}
required: [device_id, value, unit, timestamp]
additionalProperties: false
```

```python
import yaml
from jsonschema import Draft202012Validator

with open("telemetry.schema.yaml", "r", encoding="utf-8") as f:
    schema = yaml.safe_load(f)

Draft202012Validator.check_schema(schema)       # the schema itself is valid
validator = Draft202012Validator(schema)

message = {"device_id": "hum-sensor-02", "value": 48.7, "unit": "%RH", "timestamp": 1727535600}
print(validator.is_valid(message))              # Output: True
```

> In YAML `%RH` needs to be quoted because `%` at the beginning of a value is a reserved character.

---

## F.11 Beyond JSON Schema: typed models

JSON Schema is language-independent and well suited as a **shared contract** between services written in different languages.
Inside a Python application it is also common to validate data by converting it into **typed objects**:

- [`pydantic`](https://docs.pydantic.dev/): define a class with type hints; it validates, converts types and applies
  defaults (widely used by FastAPI). It can also export a JSON Schema from the model.
- `dataclasses` + manual checks: no dependencies, good for small cases.

```python
from pydantic import BaseModel, Field   # pip install pydantic

class Telemetry(BaseModel):
    device_id: str
    value: float
    unit: str
    timestamp: int = Field(ge=0)

m = Telemetry.model_validate_json('{"device_id": "temp-sensor-01", "value": 21.5, "unit": "Cel", "timestamp": 1727535600}')
print(m.value)   # Output: 21.5
```

---

## F.12 Best practices

Some commonly recommended practices:

- Validating **at the boundaries** of each component (incoming messages, API requests, configuration loading).
- Keeping the three levels separate: parse → validate structure → check business rules.
- Considering `additionalProperties: false` for configurations (it catches typos like `sampling_perod_ms`), while being more tolerant
  with messages if producers may add new optional fields over time.
- **Versioning** schemas and messages (e.g., a `"schema_version": 2` field or a versioned topic/URL) so that they
  can evolve without breaking existing consumers.
- Keeping schemas in **separate files** shared between producers and consumers, and using them in automated tests.
- Reporting errors with the **path** of the wrong field: it can save a lot of debugging time.

---

# G. Formats Comparison – Which Format for Which Job

This section puts the formats side by side, representing the **same data** in each of them, and provides a quick
guide to help choose among them. For completeness, the comparison also includes the IoT-oriented **SenML** and **CBOR** formats.

---

## G.1 The same configuration in different formats

A sensor configuration: device id, broker settings and a list of units.

**JSON**

```json
{
  "device_id": "temp-sensor-01",
  "enabled": true,
  "mqtt": {"host": "localhost", "port": 1883},
  "units": ["Cel", "K"]
}
```

**YAML**

```yaml
# Kitchen temperature sensor
device_id: temp-sensor-01
enabled: true
mqtt:
  host: localhost
  port: 1883
units: [Cel, K]
```

**TOML**

```toml
# Kitchen temperature sensor
device_id = "temp-sensor-01"
enabled = true
units = ["Cel", "K"]

[mqtt]
host = "localhost"
port = 1883
```

**CBOR** (binary, shown as hex – 77 bytes vs 105 of the compact JSON)

```text
a4 69 64 65 76 69 63 65 5f 69 64 6e 74 65 6d 70 2d 73 65 6e 73 6f 72 2d 30 31 67 65 6e 61 62 6c
65 64 f5 64 6d 71 74 74 a2 64 68 6f 73 74 69 6c 6f 63 61 6c 68 6f 73 74 64 70 6f 72 74 19 07 5b
65 75 6e 69 74 73 82 63 43 65 6c 61 4b
```

CSV and SenML are generally not well suited for this kind of nested configuration data.

---

## G.2 The same measurements in different formats

Two sensors of the same device, measured at the same time.

**JSON** (custom structure)

```json
{
  "device_id": "env-node-01",
  "timestamp": 1727535600,
  "measurements": [
    {"name": "temperature", "value": 21.5, "unit": "Cel"},
    {"name": "humidity", "value": 48.7, "unit": "%RH"}
  ]
}
```

**YAML** (possible, but less common for messages)

```yaml
device_id: env-node-01
timestamp: 1727535600
measurements:
  - {name: temperature, value: 21.5, unit: Cel}
  - {name: humidity, value: 48.7, unit: "%RH"}
```

**CSV** (one row per measurement, often used for logs)

```csv
device_id,timestamp,name,value,unit
env-node-01,1727535600,temperature,21.5,Cel
env-node-01,1727535600,humidity,48.7,%RH
```

**SenML-JSON** (standard structure)

```json
[
  {"bn": "env-node-01:", "bt": 1727535600, "n": "temperature", "u": "Cel", "v": 21.5},
  {"n": "humidity", "u": "%RH", "v": 48.7}
]
```

**SenML-CBOR** (binary, integer labels: `-2` = `bn`, `-3` = `bt`, `0` = `n`, `1` = `u`, `2` = `v`)

```text
[{-2: "env-node-01:", -3: 1727535600, 0: "temperature", 1: "Cel", 2: 21.5}, {0: "humidity", 1: "%RH", 2: 48.7}]
```

---

## G.3 Feature comparison

The table summarizes typical characteristics; actual results may vary depending on the data and the libraries used.

| | JSON | YAML | TOML | CSV | SenML | CBOR |
|---|---|---|---|---|---|---|
| Encoding | text | text | text | text | JSON / CBOR / XML | **binary** |
| Human readable | ✅ | ✅✅ | ✅✅ | ✅ | ✅ (JSON) | ❌ |
| Comments | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Nesting | ✅ | ✅ | ✅ (can be verbose) | ❌ | flat records | ✅ |
| Explicit types | basic | implicit | ✅ (+ dates) | ❌ (all strings) | fixed fields | rich (bytes, tags) |
| Size | medium | medium | medium | usually small (tables) | usually small | usually smallest |
| Schema options | JSON Schema | JSON Schema | JSON Schema | – | defined by RFC | CDDL / JSON Schema |
| Python support | `json` (built-in) | `PyYAML` | `tomllib` (built-in, read) | `csv` (built-in) | `json` / `cbor2` | `cbor2` |
| Common IoT uses | messages, APIs, telemetry | configuration, deployment | app settings | logs, datasets | sensor measurements | constrained devices, CoAP |

---

## G.4 Decision guide

A simplified guide based on common practice. Real projects can have good reasons to make different choices
(existing tools, team habits, interoperability requirements).

```text
Who usually writes / reads the data?
│
├─ Humans (configuration)
│     ├─ deeply nested, or expected by the tool (Docker, K8s, CI) ──▶ often YAML
│     └─ mostly key/value app settings, Python project ─────────────▶ often TOML
│
└─ Machines (data exchange)
      ├─ messages, events, commands, REST APIs (common default) ────▶ often JSON
      ├─ tabular data, logs, datasets for analysis ─────────────────▶ often CSV
      ├─ sensor measurements to be shared across vendors ───────────▶ consider SenML
      │        └─ on constrained devices / CoAP ────────────────────▶ consider SenML-CBOR
      └─ constrained device or network, binary data ────────────────▶ consider CBOR
```

Whatever the format, it is generally good practice to define a **schema**, **validate** data at the boundaries
([Data Validation](#f-data-validation)) and **document** conventions (units, time format, field names).

---

## G.5 Python cheat sheet

| Format | Install | Read | Write | File mode |
|--------|---------|------|-------|-----------|
| JSON | built-in | `json.load(f)` / `json.loads(s)` | `json.dump(obj, f)` / `json.dumps(obj)` | text |
| YAML | `pip install pyyaml` | `yaml.safe_load(f)` | `yaml.safe_dump(obj, f, sort_keys=False)` | text |
| TOML | built-in (read) / `pip install tomli-w` | `tomllib.load(f)` | `tomli_w.dump(obj, f)` | **binary** |
| CSV | built-in | `csv.DictReader(f)` | `csv.DictWriter(f, fieldnames)` | text, `newline=""` |
| SenML | – | `json.loads(s)` + resolve records | `json.dumps(pack)` | text |
| CBOR | `pip install cbor2` | `cbor2.load(f)` / `cbor2.loads(b)` | `cbor2.dump(obj, f)` / `cbor2.dumps(obj)` | **binary** |

---

# References

- JSON - [RFC 8259](https://www.rfc-editor.org/rfc/rfc8259), [json.org](https://www.json.org/)
- YAML - [yaml.org](https://yaml.org/), [PyYAML documentation](https://pyyaml.org/wiki/PyYAMLDocumentation)
- TOML - [toml.io](https://toml.io/), [Python tomllib module](https://docs.python.org/3/library/tomllib.html)
- CSV - [RFC 4180](https://www.rfc-editor.org/rfc/rfc4180), [Python csv module](https://docs.python.org/3/library/csv.html)
- SenML - [RFC 8428](https://www.rfc-editor.org/rfc/rfc8428), [IANA SenML registry](https://www.iana.org/assignments/senml/senml.xhtml)
- CBOR - [RFC 8949](https://www.rfc-editor.org/rfc/rfc8949), [cbor.io](https://cbor.io/), [cbor.me](https://cbor.me/) (online decoder)
- JSON Schema - [json-schema.org](https://json-schema.org/), [python-jsonschema](https://python-jsonschema.readthedocs.io/)
