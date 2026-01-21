+++
date = '2026-01-19T20:45:59-08:00'
draft = false
title = 'Scratching the Lottery Ticket'
description = 'Every graphics card is unique, not in a motivational poster way, but in a physics way. I documented my RTX 4090 undervolt journey to find out where my chip stands.'
summary = 'Every graphics card is unique, not in a motivational poster way, but in a physics way. I documented my RTX 4090 undervolt journey to find out where my chip stands.'
tags = ['hardware', 'gpu', 'nvidia', 'undervolting', 'overclocking']
+++

Every graphics card is unique. Not in a 'every GPU is beautiful' way, but in a physics way.

## Born from Sand

Your RTX 4090 started as sand. Actual sand, refined into silicon so pure that only one in a billion atoms is an impurity. That silicon gets sliced into wafers thinner than a human hair, then blasted with ultraviolet light through masks that print transistors smaller than a virus.

The AD102 chip in a 4090 has **76 billion transistors**. Each one is about 4 nanometers wide. For perspective: a COVID virus is 100nm. A strand of DNA is 2nm. We're building structures at the scale of molecules, billions of them, and expecting them all to work.

They don't all work. Not perfectly, anyway.

## The Lottery

As transistors shrink, quantum effects creep in. Electrons tunnel through barriers they shouldn't. Tiny variations in the manufacturing process, a few atoms here, a slight temperature fluctuation there, create chips that are electrically unique.

Some chips conduct efficiently at low voltages. Others need more juice to stay stable. Some run hot. Some boost higher. **Same assembly line, same design, different results.**

This is the silicon lottery.

NVIDIA bins their chips: the best ones go into flagship products, the mediocre ones get some cores disabled and become lower-tier cards, the worst ones get recycled. But even among the "winners," there's enormous variation.

## The Second Life of Failed Silicon

Here's where it gets wild. NVIDIA designs multiple GPU dies for each generation:

| Die | Transistors | Intended For |
|-----|-------------|--------------|
| AD102 | 76 billion | RTX 4090 |
| AD103 | 46 billion | RTX 4080 |
| AD104 | 36 billion | RTX 4070 |

But what happens when an AD102 comes off the line with defects in some of its 144 compute clusters? It can't be a 4090. Is it garbage?

No. NVIDIA disables the broken sections and sells it as something else.

In 2024, people started finding RTX 4070 Ti Supers with *AD102 dies inside*, the same 76-billion-transistor chip from the RTX 4090, but with **54% of its cores disabled**. These "failed" 4090 chips got a second life as mid-range cards. Similarly, RTX 4070s have been spotted with AD103 silicon (the 4080's chip), running with 43% of its cores turned off.

Your RTX 4070 might literally contain a 4080 chip that didn't make the cut. Your 4070 Ti Super might be a lobotomized 4090. You'd never know without checking GPU-Z.

This is the silicon lottery at scale: not just "how good is my chip," but "what chip do I actually have?"

## How to Find Out Where You Stand

So how do you actually measure your chip's quality? You can't just look at benchmark scores. A "good" score might be hiding inefficiencies, and a "bad" score might be thermal throttling, not silicon limits.

### This Works for Any GPU

The methodology below applies to virtually any modern graphics card, not just the RTX 4090. The specific numbers change, but the process is identical:

| What Changes | NVIDIA | AMD |
|--------------|--------|-----|
| Tuning tool | MSI Afterburner | MSI Afterburner or Radeon Software |
| Typical voltage range | 850-1100mV | 900-1150mV |
| Clock targets | Varies by GPU tier | Varies by GPU tier |
| Memory OC headroom | Usually +500 to +1500 MHz | Usually +50 to +150 MHz (GDDR6) |

**What stays the same:**
- Establish a stock baseline first
- Track effective clocks, not just reported clocks
- Calculate clock stretch to find hidden inefficiencies  
- Test in phases: memory → voltage → combined
- Stress test for stability, not just benchmarks
- Your optimal settings will be unique to your chip

Whether you have a GTX 1660, RX 7800 XT, or RTX 4090, the silicon lottery affects you. The only way to know where your chip stands is to test it.

Here's the methodology I used to evaluate my PNY RTX 4090:

### Why PNY Verto?

When I started researching undervolt settings for my card, I found plenty of results for ASUS, MSI, Gigabyte, but almost nothing for the PNY Verto edition. It's one of the more affordable 4090s, which means plenty of people own one, yet the community knowledge just isn't there.

So I documented everything. If you've got a PNY Verto 4090 and wondered how it stacks up in the silicon lottery, this is for you.

### Step 1: Establish a Baseline

Run your card completely stock. No overclocks, no undervolts. Use a consistent, repeatable benchmark (I used 3DMark Steel Nomad) and log sensor data with HWiNFO64.

Key metrics to capture:
- **GPU Clock** (reported) - what the card claims it's running
- **GPU Effective Clock** - what it's *actually* running
- **Power draw** - at high load (>90% GPU utilization)
- **Temperatures** - edge and hotspot

### Step 2: Calculate Clock Stretch

This is the secret metric most people ignore. **Clock stretch** is the gap between reported and effective clocks:

```
Clock Stretch % = (Reported Clock - Effective Clock) / Reported Clock × 100
```

A chip hitting power or thermal limits will report high clocks but actually run lower. This "stretching" is the GPU lying to you. High stretch (>2-3%) means your card is struggling.

### Step 3: Test Everything

I ran 15+ configurations across three categories: memory overclocks, core undervolts, and combinations of both. Here's the full dataset:

**Phase 1: Memory Overclock (stock voltage)**

| Config | 3DMark Score | vs Stock | Power | Notes |
|--------|--------------|----------|-------|-------|
| Stock | 8816 | - | 434W | Baseline |
| +1000 MHz mem | 8965 | +1.7% | ~400W | Stable |
| +1500 MHz mem | **9105** | **+3.3%** | ~400W | **Best score** |
| +1750 MHz mem | 9056 | +2.7% | ~400W | Regression - memory errors |

**Finding:** Memory OC sweet spot is +1500 MHz. Going higher causes error correction that tanks performance.

**Phase 2: Core Undervolt (stock memory)**

| Config | 3DMark Score | vs Stock | Power | Notes |
|--------|--------------|----------|-------|-------|
| 1000mV @ 2850 MHz | 8555 | -3.0% | ~390W | Too aggressive |
| 975mV @ 2800 MHz | 8748 | -0.8% | ~370W | Stable but slower |
| 950mV @ 2750 MHz | - | - | - | Crashed |
| 950mV @ 2730 MHz | 8699 | -1.3% | ~355W | Stable |
| 925mV @ 2700 MHz | 8593 | -2.5% | ~340W | Max efficiency |

**Finding:** Pure undervolts trade performance for power savings. My chip is stable at 950mV but only up to ~2730 MHz. The 2750 MHz crash tells me where the silicon limit is.

**Phase 3: Combo (undervolt + memory OC)**

| Config | 3DMark Score | vs Stock | Power | Stability |
|--------|--------------|----------|-------|-----------|
| 950mV/2750 MHz + 1500 mem | 8917 | +1.1% | ~355W | ❌ Failed stress test |
| 950mV/2750 MHz + 1750 mem | 8911 | +1.1% | ~383W | ❌ Failed stress test |
| **950mV/2730 MHz + 1500 mem** | **8948** | **+1.5%** | **394W** | ✅ **Passed 20 loops** |

**Finding:** 2750 MHz looked fine in short benchmarks but failed extended stress testing. Backing off 20 MHz to 2730 MHz provided the stability margin needed.

### Step 4: Stress Test the Finalists

Quick benchmarks lie. A config might score well once but crash under sustained load. I ran 20-loop Steel Nomad stress tests (~21 minutes each) on promising candidates:

| Config | Stability % | Loops | Result |
|--------|-------------|-------|--------|
| 950mV/2750 MHz + 1500 mem | - | 8/20 | ❌ Crashed |
| 950mV/2750 MHz + 1750 mem | - | 12/20 | ❌ Crashed |
| **950mV/2730 MHz + 1500 mem** | **98.9%** | **20/20** | ✅ **Passed** |

### Step 5: Analyze the Winner

Here's the detailed comparison between stock and my final config:

| Metric | Stock | 950mV/2730/+1500mem | Delta |
|--------|-------|---------------------|-------|
| 3DMark Score | 8816 | 8948 | **+1.5%** |
| Reported Clock | 2553 MHz | 2714 MHz | +161 MHz |
| **Effective Clock** | 2480 MHz | 2693 MHz | **+213 MHz** |
| Power (high load) | 434W | 394W | **-40W** |
| Clock Stretch | 2.87% | 0.77% | **-2.1%** |
| Edge Temp | ~72°C | ~68°C | -4°C |
| Hotspot Temp | ~85°C | ~81°C | -4°C |

The undervolted config runs **higher effective clocks** on **40 fewer watts** with **nearly 4x less clock stretch**.

### What This Tells Me

My card isn't a lottery winner. Stock boost clocks (~2550 MHz reported) are below average for a 4090, most hit 2700-2800+. The 2.87% clock stretch confirms it: stock, this chip is power-limited and struggling.

But it responds well to undervolting. At 950mV, it stops fighting itself and runs clean. The silicon isn't bad, NVIDIA's default voltage curve just isn't optimized for *this specific chip*.

**My lottery standing: ~45th percentile.** Below average stock performance, but recoverable with tuning.

### My Daily Driver

**950mV @ 2730 MHz core, +1500 MHz memory**

| vs Stock | Benefit |
|----------|---------|
| +1.5% performance | Faster in games |
| -40W power | Lower electricity bill, less connector stress |
| -4°C cooler | Quieter fans |
| 0.77% stretch | Card running clean |

Not the highest-scoring config (that's stock voltage +1500 mem at 9105), but the best balance of performance, efficiency, and thermals. For max FPS at any cost, I'd run stock +1500 mem. For daily use, the undervolt wins.

#### A Note on the 12VHPWR Connector

Remember the RTX 4090 melting connector fiasco? The 16-pin 12VHPWR connector (12 power pins + 4 sense pins) pushing 450W+ caused fires when connectors weren't fully seated or wires fatigued over time. NVIDIA and connector manufacturers took the blame, but physics was the real culprit: that much power through such a compact connector leaves zero margin for error.

Undervolting helps. My config pulls ~394W instead of ~434W, that's 40W less current stressing those pins. It won't save a badly seated connector, but it reduces thermal stress on a component that's already operating near its limits.

One more reason to tune your card: **longevity**. Less power, less heat, less stress on the weakest link in the system.

---

*This is part of a series documenting my RTX 4090 undervolt/overclock journey. The chip lottery gave me a ~45th percentile sample. Through testing, I found that 950mV @ 2730MHz with +1500MHz memory delivers better performance than stock while saving 40W. Your results will vary—that's the whole point.*
