# HTND 2.19.0.1

Patch on 2.19-cryptonoob-rc. The ledger rule is unchanged.

## Change

The headers-proof refuse now runs before the IBD flag is set.

A peer whose blue score is past pruning depth, against a live and mature local chain, no longer enters IBD. The node declines and stays available for block relay and for serving sync. Other IBD paths are unchanged.

Previously the flag was set first and cleared after the refuse. While it was set, the node reported itself as syncing. Block submission waited, and IBD locator requests from other peers were answered as not found.

## Scope

This does not change the UTXO commitment check, the coinbase check, or the pruning-point import rule. A node that is behind still syncs. A node on genesis still syncs. The refuse applies only where the previous headers-proof gate already declined.

## Build

The binary reports 2.19.0.1. Go 1.27.1 or later.

Linux:

    go install -ldflags="-s -w" --tags="pebblegozstd" . ./cmd/...

Windows needs TDM-GCC and the deadlock tag:

    go install -ldflags="-s -w" --tags="deadlock pebblegozstd" . ./cmd/...
