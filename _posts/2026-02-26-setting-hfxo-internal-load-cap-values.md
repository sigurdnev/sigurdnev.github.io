---
layout: post
title: "Setting HFXO internal load cap values"
date: 2026-02-25 12:00:00 +0000
tags: [Clocking, HFXO, nRF]
---
Correct HFXO load capacitor settings are important for startup reliability and frequency accuracy.

## Why this matters

- Incorrect load values can increase startup time.
- Frequency offset can impact BLE and other radio performance.
- Stable clock behavior improves overall system robustness.

## Practical workflow

1. Start from board or reference design recommendations.
2. Configure internal HFXO load cap values in your project configuration.
3. Validate startup and RF behavior on real hardware across temperature and voltage ranges.

## Verification checklist

- Confirm consistent oscillator startup.
- Check protocol timing/radio metrics in your target scenario.
- Document final values and rationale for future maintenance.

## Example code

```c
#define HFXO_LOAD_CAP_PF 15.0f

#include <hal/nrf_oscillators.h>

uint32_t hfxo_cap_value = NRF_OSCILLATORS_HFXO_CAP_CALCULATE(NRF_FICR, HFXO_LOAD_CAP_PF);
uint32_t intcap_before = NRF_OSCILLATORS->XOSC32M.CONFIG.INTCAP;
printf("HFXO cap value: %u\n", hfxo_cap_value);
printf("XOSC32M.CONFIG.INTCAP before set: %u\n", intcap_before);

nrf_oscillators_hfxo_cap_set(NRF_OSCILLATORS, true, hfxo_cap_value);

uint32_t intcap_after = NRF_OSCILLATORS->XOSC32M.CONFIG.INTCAP;
printf("XOSC32M.CONFIG.INTCAP after set: %u\n", intcap_after);
```
