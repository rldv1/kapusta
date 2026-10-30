# kapusta

[Русский](README.md)

An extremely raw and experimental monolithic 2G + 3G + LTE + NR + CDMA + NB-IoT base station running on a 1T1R USRP B210.

More information will come later.

## Requirements

| | Minimum |
|---|---|
| Radio | Ettus USRP B210 (or B200/B200mini) on USB 3 |
| Reference | GPSDO with 10 MHz + PPS outputs (required) |
| CPU | A fast x86-64 with AVX2/FMA, 8 physical cores for all RATs at once, e.g. Ryzen 7000 series or newer |
| OS | Linux x86-64 (glibc 2.39+), a PREEMPT_RT or lowlatency kernel is recommended |
| SIM | [Programmable USIMs (Milenage K/OPc)](https://www.gialer.com/collections/sim-card-program-kit-1/products/gialer-sim-card-program-kit-sim-card-tools-accessories-include-1-multi-sim-card-reader-5pcs-programmable-usim-cards-3-in-1-card-adapter-kit-newest-grsim-software-programer-tool) or a [programmable eSIM](https://simlessly.com) |

### GPSDO
A GPSDO is required; without one the project will not run reliably. I recommend [this one](https://www.leobodnar.com/shop/index.php?main_page=product_info&cPath=107&products_id=393).

## Running

Nothing to install: the radio stacks, the core network, IMS, the subscriber database and the web panel are all inside one file.

1. Connect attenuators or antennas, feed the GPSDO's 10 MHz + PPS into the USRP and plug the USRP into USB 3.0.
2. Download the latest binary from [Releases](../../releases) and run it as root:
   ```sh
   wget https://github.com/rldv1/kapusta/releases/download/v1.0.0/kpstv1.0.0-linux-amd64
   chmod +x kpstv1.0.0-linux-amd64
   sudo ./kpstv1.0.0-linux-amd64
   ```
3. On the first start, do the initial setup! The console shows a QR code with the admin panel and its password. Set up the network and add subscribers.
4. Press Ready once your frequency map is done **(do not overlap the frequencies of real operators!)**, and the air goes live.

All settings, the subscriber database and the logs live in the `kapusta-data/` folder next to the binary; nothing else is installed on the system. To move the node to another machine, copy the binary together with that folder. To stop: `Ctrl+C` or `stop` in the console.

### Operator console

| Command | Effect |
|---|---|
| `status`, `health`, `ue list` | Active RATs, USRP/GPSDO lock, CPU load, attached phones |
| `rat on <rat>`, `rat off <rat>` | Switch `gsm`, `umts`, `lte`, `nr`, `nbiot`, `cdma` on or off live |
| `sub add <imsi> <k> <opc> [msisdn]`, `sub del <imsi>`, `sub list` | Subscribers in the HSS, HLR and UMTS AuC |
| `spectrum`, `spectrum set <rat> <center_mhz> [<bw_mhz>\|<profile>] [restart]` | Carriers, moving and resizing one RAT live (`lte umts umts_c1 gsm cdma nbiot`) |
| `umts tx_gain <db>`, `rf gain <rat> <tx_db> [rx_db]` | Radio parameters |
| `log filter <tag...>`, `log mute <tag>`, `log unmute <tag>`, `log level info\|debug` | Log filters |
| `stop`, `exit` | Stop the radio |

## Contact

https://t.me/rldv1
