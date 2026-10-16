# V-JEPA kitchen

The continuation of [microwave-from-human-video](https://github.com/HanshangZhu/microwave-from-human-video):
the human video trains the world model; the digital twin only examines it.

## Why

In the first project, three RGB-D videos of me putting a pot into a microwave became a
scripted-solver dataset and an imitation policy in a measured digital twin. Its clearest
negative result: wherever a scripted oracle exists, human motion used as *action labels*
loses to it (59 % vs 88 % at matched demonstration counts). Its clearest positive one: the
video carries what the simulator cannot produce -- how a real door, pot and room look and
move under manipulation -- and that needs no action labels to learn.

So here the human data changes sides of the pipeline:

1. **Scale ego-motion video**: head- or chest-mounted RGB(-D), many takes, many objects and
   rooms; the wrist pose tracked as the action proxy.
2. **Train the video head of a world-action model on it** (V-JEPA 2-style predictor, or the
   video branch of a WAM); where data are few, the object-centric variant -- predict pot pose
   and door angle from wrist motion -- is the minimal instance.
3. **Align the action head with the robot's own embodiment**: SFT on robot demonstrations,
   then RL in the twin, where success is measurable and the oracle is a critic, not a
   demonstrator.
4. **The simulator is the exam**: the twin and its Gaussian-splat room score the model on
   starts, rooms and bodies the video never showed.

## Plan

| step | what | status |
|---|---|---|
| 0 | object-centric world model on the three existing takes (state: pot pose + door angle; action: wrist pose); evaluated in the twin against simulated object states | next |
| 1 | ego-video capture at scale (head-mounted RealSense / phone), wrist tracking | planned |
| 2 | V-JEPA 2 / WAM video head fine-tuned on the captures; prediction quality vs horizon | planned |
| 3 | action head: SFT on simulator demonstrations, RL in the twin | planned |
| 4 | whole-body G1 in Isaac Lab on an existing whole-body controller | planned |

Assets, twin, registration and per-frame object extraction come from the first repository
(`recon/`, `sim/`); the room splat and the three takes are recordings of my home and are
shared on request (zcabhz4@ucl.ac.uk).

*Started 17 October 2026.*
