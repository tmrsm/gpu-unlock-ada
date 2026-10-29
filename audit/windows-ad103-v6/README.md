# Exact successful artifact: INTEGRATED-UNLOCK-v6

This directory records the **actual Windows UEFI program that ran** on the
ASUS B550-F / RTX 4070 Ti SUPER test machine, together with the logs and
same-boot validation from that run. It is not a rebuilt variant and not the
opt-in port that was proposed separately.

## Plain-language summary

- `INTEGRATED-UNLOCK-v6.EFI` is the program that actually booted from the USB, took the **U** key choice, unlocked the GPU, and handed off to Windows.
- That boot had both 1920x1080 displays working, no NVIDIA device error, zero numerical failures, and roughly twice the stock rate for the unlocked tensor modes.
- `evidence/HND0022.TXT`, `evidence/HND0023.TXT` and `validation/` are the records from that boot.
- Limit: this is **one successful boot**. It does not prove long-term stability, every workload, or repeatability after a cold boot. A reboot loses the unlock; rerun the USB flow. No automatic persistence and no VBIOS/firmware modification is included.

## Artifact

| file | value |
|---|---|
| `INTEGRATED-UNLOCK-v6.EFI` | 129,536 bytes, SHA-256 `8aa634c408210133cc6aff5f4fb746c91f650e78a586ed6c4e9ce845b29dc6c1` |
| `INTEGRATED-UNLOCK-v6.manifest.json` | exact source hashes, asset-manifest hash, no signing/execution claim |
| `evidence/HND0021.TXT` | control: no payload/FLR/DMA reproduced Code 12 |
| `evidence/HND0022.TXT` | VGA restore control: both displays, GPU OK |
| `evidence/HND0023.TXT` | full transaction + post-FLR VGA restore + Windows handoff |
| `evidence/analysis-0022.json`, `analysis-0023.json` | structured summaries |
| `validation/` | same-boot correctness, issue-rate, result, boot events |

## What ran

1. Boot the pinned USB, press **U** once.
2. The program performs the full recorded transaction: FWSEC, SEC2/payload,
   selectors, FLR, post-FLR flush, VGA `0x18` restore, claim/DMA release and
   Windows handoff.
3. Windows started directly. Three numerical runs had zero failures; matched
   instruction rates were 2.067x f16/f32 and 2.068x bf16/f32 over the stock
   baseline, while controls stayed near 1.0x.
4. The same Windows boot was validated; no recovery reboot was used.

## Verification

```
sha256sum INTEGRATED-UNLOCK-v6.EFI
# 8aa634c408210133cc6aff5f4fb746c91f650e78a586ed6c4e9ce845b29dc6c1
```

`INTEGRATED-UNLOCK-v6.manifest.json` pins the exact source-file hashes used by
the build. The source tree and the asset manifest are kept in the audit
workspace; they are not duplicated here. The USB's `GPU-UNLOCK` firmware
assets, the captured PROM and the Windows boot image are **not** included.

## Limits

- One successful boot; not proof of long-term stability, all workloads,
  repeatability across cold boots, or universal safety of the firmware VGA
  mutation.
- The unlock is volatile. A reboot/power cycle loses it; rerun the USB flow.
- No automatic boot persistence and no VBIOS/firmware modification is
  included.
- The program and logs are specific to this board/GPU/driver combination.
- This is a record of the run, not a new build.
