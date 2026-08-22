---
title: TIL about RAM training
author: Federico D'Eredità
pubDatetime: 2026-07-25
slug: til-about-ram-training
featured: false
draft: false
tags:
  - til
  - hardware
description:
  A new RAM stick wouldn't boot and threw POST beep codes, a comment blamed "RAM training", turns out that's a real BIOS/memory-controller calibration step, though in this case the actual culprit was a bad stick.
---

## Context

Bought a new RAM stick for my Lenovo laptop. It didn't work, the laptop wouldn't boot, POST gave error beep tones. Posted about it on [r/Lenovo](https://www.reddit.com/r/Lenovo/comments/1v5evdg/comment/ozld9de/). A commenter suggested the issue could be "RAM training." I assumed it was a joke 😂

## Fact

As the curious person I am, I still give it a go and searched about it, turns out it's not a joke! RAM training (memory training) is a real, low-level procedure the memory controller runs during POST. On first boot with new/unfamiliar modules, the BIOS writes test patterns and iterates on timings, voltages, and signal parameters until it finds a stable configuration for that specific stick. This is why a new RAM install can trigger unusually long POST times or repeated automatic reboots the first time, the system is calibrating, not necessarily failing.

## Consequence

In my case, training wasn't the explanation: the POST beep codes pointed to a genuinely bad stick, confirmed after getting a refund. The replacement stick booted straight away, no training delay, no error tones.
Furthermore when a training happens, at least on Lenovo laptops, a message is shown on the display.

## Takeaway

Long POST / reboot loops after a RAM swap aren't automatically a red flag — could just be training. But don't stop there: check the beep/error codes first. If the board is reporting a specific memory fault, it's a bad stick, not training.

Source: [How-To Geek – What Is DDR5 Memory Training?](https://www.howtogeek.com/what-is-ddr5-memory-training/)
