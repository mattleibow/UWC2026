# TorqueBay product specification

## Kickoff prompt

Select the **Dodgy Beautiful App** custom agent and paste:

```text
Build the application described in docs/app-spec.md.

Deliver the complete, polished, working product rather than a prototype. Make
reasonable product and implementation decisions without waiting for follow-up
questions.
```

## Summary

TorqueBay is a mobile pit-crew companion for a fictional inter-university robot
arena league.

University teams use it to configure a robot for an event, run simulated
test-bench checks, attach technical-inspection evidence, reserve an inspection
slot, and submit their loadout to the tournament roster.

The product should feel like a real application a student robotics league might
commission. It is not a robot-control system and does not provide instructions
for constructing real weapons or hazardous machinery.

## Product vision

Preparing a competition robot involves many decisions, multiple team members,
limited inspection slots, and a lot of last-minute pressure. TorqueBay gives a
team one clear place to prepare its loadout and understand whether it is ready
for competition.

The experience should combine:

- the clarity of a professional engineering tool;
- the excitement of a tournament pit lane;
- the immediacy of live telemetry;
- and the approachability of a polished student product.

## Target users

### Team captain

Owns the team's tournament entry and makes final submission decisions.

### Pit engineer

Collaborates on tuning, runs test-bench checks, and prepares inspection
evidence.

### Tournament inspector

Reviews submitted loadouts and grants official technical clearance.

For demonstrations, the app uses synthetic local accounts:

- `alice` / `P@55w0rd!` — captain of Team Voltage;
- `avery` / `P@55w0rd!` — pit engineer for Team Voltage;
- `bob` / `P@55w0rd!` — captain of rival Team Torque.

## Core experience

The main journey is **prepare a robot for competition**:

1. Sign in.
2. Browse the tournament Pit Board.
3. Open a team robot in the Loadout Workbench.
4. Select its chassis and arena module.
5. Adjust its tuning.
6. Review calculated weight, power, heat, stability, and league eligibility.
7. Run the simulated Test Bench.
8. Attach an inspection photo.
9. Choose the match date and technical-inspection slot.
10. Run an automated readiness pre-check.
11. Submit the loadout.
12. Return to the Pit Board to see its current status.

Team members can later reopen the same robot and continue preparing it.

## Main areas

### Sign in

Users sign in to their team account with a username and password.

The Debug workshop build may provide small **Fill Alice**, **Fill Avery**, and
**Fill Bob** helpers to reduce typing during a presentation. These helpers only
populate the username and password fields. They must not authenticate the user,
navigate, create a session, or bypass the normal Sign In action and API.

The sign-in screen must still look and behave like a credible product,
including loading, invalid-credential, retry, focus, keyboard, and password
visibility behavior.

### Pit Board

The Pit Board is the tournament overview.

It shows:

- the signed-in team;
- its robots and their readiness;
- upcoming inspection times;
- the wider tournament roster;
- and clear draft, pending, cleared, and conflict states.

Users can search, filter, and progressively load roster entries.

### Loadout Workbench

The Workbench is the centerpiece of the application.

Users can:

- name the robot;
- choose a chassis class;
- choose a safe fictional arena module;
- adjust drive torque, capacitor boost, and armor tuning;
- see the resulting metrics and league eligibility;
- run a simulated test;
- attach inspection evidence;
- schedule inspection;
- and submit the loadout.

The screen should feel tactile and responsive without becoming a CAD tool or a
full game.

### Test Bench

The Test Bench provides a short simulated diagnostic run.

It displays animated values such as:

- bus voltage;
- motor temperature;
- vibration;
- and RPM.

The results are deterministic and based on the selected tuning. They are
fictional game-like values, not real robot-engineering guidance.

### Technical inspection

Each tournament entry needs:

- a scale-and-safety inspection photo;
- a match date;
- an available technical-inspection slot;
- and a successful readiness pre-check.

An automated pre-check helps the team understand likely problems, but only a
tournament inspector can grant official clearance.

## Robot configuration

### Chassis classes

- Featherweight
- Midweight
- Heavyweight

### Fictional arena modules

- Magnetic Grappler
- Flipper Pad
- Gyro Stabilizer
- Omni-Traction Ring

### Tuning controls

- Drive Torque
- Capacitor Boost
- Armor Plating

Changes should immediately update understandable indicators for:

- total mass;
- peak current draw;
- thermal load;
- stability;
- and league eligibility.

## Visual direction

The design language is **paddock telemetry industrial**.

- Dark carbon and charcoal surfaces.
- High-visibility safety amber.
- Telemetry cyan.
- Clear green legal/ready states.
- Restrained red overload/error states.
- Large readable values and strong visual hierarchy.
- Polished cards, meters, segmented bars, and status chips.

The visual design must remain accessible, adapt to different screen sizes and
orientations, support dynamic text, and work in light and dark themes.

## Experience states

Every data-driven area should deliberately support:

- loading;
- content;
- empty;
- validation error;
- network error;
- permission denied;
- submission in progress;
- successful submission;
- edit conflict;
- and retry.

User-entered work must not disappear merely because a request fails.

## Collaboration

Members of the same team can prepare the same robot.

If two people edit an outdated version, the app should explain that the loadout
changed rather than silently discarding one person's work.

Users from rival teams can see appropriate public roster information but cannot
change or inspect private loadout details.

## Tournament scale

The application should remain useful for a large historical roster, not just
the handful of robots visible in the initial demonstration.

Search and filtering should feel immediate, images should be appropriately
sized, and the app should load more entries as needed rather than downloading
the whole archive.

## Product principles

- Make readiness easy to understand.
- Preserve work during interruptions and failures.
- Distinguish team estimates from official league decisions.
- Keep private team details private.
- Make collaboration explicit.
- Prefer clear recovery over silent failure.
- Show enough technical information to feel authentic without pretending to
  control real hardware.

## Non-goals

TorqueBay does not include:

- real robot hardware control;
- weapon construction or engineering instructions;
- a combat physics engine;
- a playable battle game;
- Bluetooth or serial-device pairing;
- GPS or augmented reality;
- chat;
- live multiplayer;
- or real tournament infrastructure.

## Success criteria

The product is successful when a new user can:

- understand the Pit Board without explanation;
- configure a robot and understand how tuning changes its indicators;
- run and stop a simulated Test Bench;
- attach inspection evidence;
- select valid event timing;
- submit a loadout and understand its resulting state;
- recover from errors without losing work;
- and collaborate without silently overwriting another team member.
