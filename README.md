# Windows AD103 GPU unlock (RTX 4070 Ti SUPER, `10de:2705`)

This fork focuses on the **Windows UEFI** one-shot unlock path for the AD103
RTX 4070 Ti SUPER (`10de:2705`). It is not a Linux module build. The upstream
Linux project is retained for provenance and credits; see
[Upstream and credits](#upstream-and-credits).

## Verified result (one boot)

HND0023 completed the full transaction (FWSEC, SEC2/payload, selectors, FLR,
post-FLR VGA restore) and handed off to Windows directly:

- both physical **1920x1080** displays active, NVIDIA problem 0
- three same-boot numerical runs: **zero failures**
- matched rates: FP16/FP32 **2.067x**, BF16/FP32 **2.068x**; FP16/FP16,
  INT8, `dp4a` and scalar FP32 near **1.0x**

The exact EFI (SHA-256
`8aa634c408210133cc6aff5f4fb746c91f650e78a586ed6c4e9ce845b29dc6c1`), the
HND0021/22/23 logs and the same-boot validation are in
[`audit/windows-ad103-v6/`](audit/windows-ad103-v6/README.md).

## Scope and layout

- [`audit/windows-ad103-v6/`](audit/windows-ad103-v6/README.md) — the exact
  EFI, evidence logs, analysis, validation and checksums from the run.
- [`windows/ad103-2705/`](windows/ad103-2705/README.md) — the Windows AD103
  port source and build notes.
- [`docs/`](docs/) — reports and the preserved upstream Linux README at
  [`docs/upstream-linux-readme.md`](docs/upstream-linux-readme.md).
- `kernel/`, `debian/`, packaging and the Linux quick start are upstream Linux
  material kept for provenance. They are **not** part of this fork's supported
  Windows path.

## How it runs

1. Boot the pinned USB from the firmware boot menu.
2. Press **U** once at the `INTEGRATED GPU UNLOCK` screen.
3. Wait at least **120 seconds**. If Windows starts, keep that same boot for
   validation. If it stays blank or does not reach Windows, power off, remove
   the USB and boot normally; do not repeat the attempt.
4. The exact USB firmware assets, captured PROM and Windows image are not in
   this repository.

## Limits

- **One successful boot.** This does not prove long-term stability, all
  workloads, repeatability across cold boots, or universal safety of the
  firmware VGA mutation.
- The unlock is **volatile**. A reboot or power cycle loses it; rerun the USB
  flow. No automatic boot persistence and no VBIOS/firmware modification is
  included.
- The result is specific to this board, GPU and driver combination.

## Upstream and credits

This fork is based on
[`JustAResearcher/gpu-unlock-ada`](https://github.com/JustAResearcher/gpu-unlock-ada).
The original Linux `gpu-unlock` project, its full README and its benchmark
reports are preserved in
[`docs/upstream-linux-readme.md`](docs/upstream-linux-readme.md). The AD102
payload and booter analysis come from
[TOTORONG/AD103_win_unlocker](https://huggingface.co/TOTORONG/AD103_win_unlocker);
the AD104 Windows reference was contributed by Latence.app. Upstream licences
and notices are retained.
