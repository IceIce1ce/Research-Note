<h2>1. File format</h2>
$\textbf{<frame>, <id>, <bb\_left>, <bb\_top>, <bb\_width>, <bb\_height>, <conf>, <x>, <y>, <z>}$. The world coordinates $\textbf{x, y, z}$ are ignored for 2D challenge and can be filled with -1.
<h2>2. Summary</h2>
<h3>2.1 MOT15</h3>
<h4>2.1.1 Train</h4>
- ADL-Rundle-6: 525
- ADL-Rundle-8: 654
- ETH-Bahnhof: 1000
- ETH-Pedcross2: 837
- ETH-Sunnyday: 360
- KITTI-13: 340
- KITTI-17: 145
- PETS09-S2L1: 795
- TUD-Campus: 71
- TUD-Stadtmitte: 179
- Venice-2: 600
<h4>2.1.2 Test</h4>
- ADL-Rundle-1: 500
- ADL-Rundle-3: 625
- AVG-TownCentre: 450
- ETH-Crossing: 219
- ETH-Jelmoli: 440
- ETH-Linthescher: 1194
- KITTI-16: 209
- KITTI-19: 1059
- PETS09-S2L2: 436
- TUD-Crossing: 201
- Venice-1: 450
<h3>2.2 MOT16</h3>
<h4>2.2.1 Train</h4>
- MOT16-02: 600
- MOT16-04: 1050
- MOT16-05: 837
- MOT16-09: 525
- MOT16-10: 654
- MOT16-11: 900
- MOT16-13: 750
<h4>2.2.2 Test</h4>
- MOT16-01: 450
- MOT16-03: 1500
- MOT16-06: 1194
- MOT16-07: 500
- MOT16-08: 625
- MOT16-12: 900
- MOT16-14: 750
<h3>2.3 MOT17</h3>
<h4>2.3.1 Train</h4>
MOT17-02-DPM: 600
MOT17-04-DPM: 1050
MOT17-05-DPM: 837
MOT17-09-DPM: 525
MOT17-10-DPM: 654
MOT17-11-DPM: 900
MOT17-13-DPM: 750
MOT17-02-FRCNN: 600
MOT17-04-FRCNN: 1050
MOT17-05-FRCNN: 837
MOT17-09-FRCNN: 525
MOT17-10-FRCNN: 654
MOT17-11-FRCNN: 900
MOT17-13-FRCNN: 750
MOT17-02-SDP: 600
MOT17-04-SDP: 1050
MOT17-05-SDP: 837
MOT17-09-SDP: 525
MOT17-10-SDP: 654
MOT17-11-SDP: 900
MOT17-13-SDP: 750
<h4>2.3.2 Test</h4>
MOT17-01-DPM: 450
MOT17-03-DPM: 1500
MOT17-06-DPM: 1194
MOT17-07-DPM: 500
MOT17-08-DPM: 625
MOT17-12-DPM: 900
MOT17-14-DPM: 750
MOT17-01-FRCNN: 450
MOT17-03-FRCNN: 1500
MOT17-06-FRCNN: 1194
MOT17-07-FRCNN: 500
MOT17-08-FRCNN: 625
MOT17-12-FRCNN: 900
MOT17-14-FRCNN: 750
MOT17-01-SDP: 450
MOT17-03-SDP: 1500
MOT17-06-SDP: 1194
MOT17-07-SDP: 500
MOT17-08-SDP: 625
MOT17-12-SDP: 900
MOT17-14-SDP: 750
<h3>2.4 MOT20</h3>
<h4>2.4.1 Train</h4>
MOT20-01: 429
MOT20-02: 2782
MOT20-03: 2405
MOT20-05: 3315
<h4>2.4.2 Test</h4>
MOT20-04: 2080
MOT20-06: 1008
MOT20-07: 585
MOT20-08: 806
<h2>3. Evaluation metrics</h2>
- IDF1: $IDF1 = \frac{2 * IDTP}{2 * IDTP + IDFP + IDFN}$
- IDP: $IDP = \frac{IDTP}{IDTP + IDFP}$
- IDR: $IDR = \frac{IDTP}{IDTP + IDFN}$
- IDTP: the longest associated trajectory matching to a groundtruth trajectory is regarded as thge GT's true ID. Then other trajectories matching to this GT is regarded as a IDFP.
- Recall: $Recall = \frac{TP}{TP + FN}$
- Precision: $Preceision = \frac{TP}{TP + FP}$
- FAR: $FAR = \frac{FP}{\sum_t1}$
- GT: the number of groundtruth trajectories.
- MT: the number of trajectories that have over 80% target tracked.
- PT: the number of trajectories that have 20% to 80% target tracked, $PT = GT - MT - ML$.
- ML: the number of trajectories that have less than 20% target tracked.
- IDs: $IDs = \sum_tids_{i, t}$
- FM: Fragmentation reflects the continousity of trajectories. When trajectories are determined, it counts all missed target in each frame.
- MOTA: $MOTA = 1 - \frac{FN + FP + IDS}{T}$, where $T = \sum_t\sum_igt_{i, t}$
- MOTP: $MOTP = \frac{\sum_{i, t}IoU_{t, i}}{TP}$
- MOTAL: $MOTA = 1 - \frac{FN + FP + log_{10}IDS}{T}$