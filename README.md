---
title: File transfer over MQTT
---

## Abstract

This document defines protocol to send files from MQTT clients to MQTT server. It is using only `PUBLISH` and `PUBACK` messages and does not require MQTT 5.0 features like topic aliases.

## Motivation

Transfering files from IoT devices to their cloud (primary use case), and from cloud to IoT devices is a common requirement for IoT applications. Frequently, they are uploading files from devices via FTP or HTTPS (e.g. to S3), but this approach has downsides:

* FTP and HTTP servers usually struggle to keep up with large number of simultaneous bandwidth-intensive connections
* packet loss or reconnect forces clients to restart the transfer
* devices which already talk MQTT need to integrate with one more SDK, address authentication and authorization, and potentially go through an additional round of security audit

Known cases of device-to-cloud file transfer:

* [CAN bus](https://en.wikipedia.org/wiki/CAN_bus) data
* Image taken by industry camera for Quality Assurance
* Large data file collected from forklift
* Video and audio data from truck cars, and video data captured by inbound unloading cameras
* Vehicle real-time logging, telemetry, messaging
* Upload collected ML logs

Known cases of cloud-to-device file transfer:

* Upload AI/ML models
* Firmware upgrades

Even though devices could already send binary data in MQTT packets, it is not trivial to guarantee reliable file transfer without clear expectations from client and server side.

The proposed solution is to use a protocol on top of MQTT to transfer files.