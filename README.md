# KV4P-F

### Radio. Reimagined.

<img width="1122" height="1402" alt="KV4P-F Radio Reimagined" src="https://github.com/user-attachments/assets/3b812498-2ad9-4105-aea0-2a3b8499b577" />


**Off-grid by design. Connected by choice.**

**KV4P-F** is an independent, open-source fork of the [KV4P-HT](https://github.com/VanceVagell/kv4p-ht) handheld amateur radio project, developed by **[Feedline Labs](https://feedlinelabs.com/)**.

We're building on a clever idea: an affordable, real RF handheld powered by an Android phone and an ESP32-based radio. Our vision adds an optional layer of real-time repeater intelligence from the **[Greyline Fabric](https://developer.feedlinelabs.com/)**—without making an internet connection a requirement to get on the air.

**Real radios. Real RF. Real activity.**

> [!IMPORTANT]
> **Independent project notice:** KV4P-F is a Feedline Labs–maintained fork, **not** an official KV4P-HT release, partnership, joint development effort, or endorsement by the original KV4P-HT creator or maintainers. We appreciate their open-source work and preserve attribution to the upstream project.

> [!NOTE]
> **Project status: early development.** This repository currently begins with the upstream KV4P-HT codebase. The Greyline Fabric integration and redesigned repeater experience described below are **planned features**, not shipped capabilities. There is no KV4P-F public release to install yet.

## Why KV4P-F?

We believe amateur radio should work when the network doesn't—and become even more useful when it does.

KV4P-F's direction is straightforward:

- **RF first.** Local radio operation should never depend on a server, an account, or an internet connection.
- **Connected when you want it.** Discover repeaters through Feedline Labs' Greyline Fabric when online.
- **Activity at a glance.** See which known repeaters are recently active, with compact channel rows and a subtle warm-yellow highlight for activity.
- **Transmit reporting by choice.** With an explicit, **off-by-default opt-in**, report a push-to-talk (PTT) event for a matched Greyline repeater as transmission begins.
- **An open foundation.** Keep the project hackable, welcoming to contributions, and connected to its open-source roots.

We're not trying to turn a handheld into a waterfall display or a cloud-dependent radio. We want a better repeater experience around **actual over-the-air communication**.

## Planned Greyline Fabric integration

| Capability | Intended behavior | Status |
| --- | --- | --- |
| Nearby repeater discovery | Retrieve repeater data from the Feedline Labs Radio API and make it available as radio channel memories | Planned |
| Live activity hints | Look up recent activity and gently highlight active repeaters in tighter, easier-to-scan rows | Planned |
| Optional PTT activity | Send one activity report on a successful PTT start **only** when the operator opted in and the selected channel matches a Greyline repeater ID | Planned |
| Offline continuity | Continue using locally saved channel memories and the underlying RF controls without a network connection | Design requirement |
| iPhone / macOS support | Explore a separate path after the Android integration; not part of the initial scope | Future exploration |

### Privacy and operator control

Our integration is intended to be **opt-in and minimally intrusive**:

1. PTT activity reporting will be **disabled by default**.
2. Only a deliberate, successful transmit-start event on a channel matched to a known Greyline repeater will qualify for reporting.
3. No report will be sent merely because the user selects a channel, monitors traffic, or presses PTT on an unmatched frequency.
4. Network reporting must run asynchronously; API outages must not delay or prevent RF operation.
5. Old PTT events should **not** be replayed after connectivity returns, since stale events would misrepresent current repeater activity.
6. Location access and repeater discovery should be transparent to the operator, with clear controls over when online lookups are performed.

The exact settings, data flow, and privacy documentation will be finalized with the implementation and reviewed before release.

### For developers: integration direction

The planned Android integration uses the [Feedline Labs Developer Radio API](https://developer.feedlinelabs.com/reference/index.html). Relevant endpoints include:

```http
GET  /v1/radio/repeaters/{latitude}/{longitude}
GET  /v1/radio/active-repeater-ids/{latitude}/{longitude}
POST /v1/radio/push-to-talk/{id}
```

The PTT endpoint requires appropriate Feedline Labs partner authorization. **Do not embed a long-lived partner API key in a distributable APK.** A narrowly scoped, revocable credential or controlled relay will be part of the production design.

The initial work is expected to be on the **Android application side**, including repeater lookup, mapping API repeater IDs to stored channel memories, activity visualization, settings, and transmit-start event handling. We are **not currently proposing changes to the RF firmware protocol** for these features.

## Roadmap

- [x] Create the independent Feedline Labs fork
- [ ] Add Greyline repeater search/import to the Android app
- [ ] Preserve Greyline repeater IDs with stored channel memories
- [ ] Add compact channel rows and subtle recent-activity highlighting
- [ ] Add a visible, off-by-default PTT activity-reporting preference
- [ ] Report qualifying PTT-start activity without affecting radio responsiveness
- [ ] Implement appropriate production API authorization and abuse protection
- [ ] Test connected and completely offline behavior with real hardware
- [ ] Publish KV4P-F builds, documentation, and release notes

This is a direction of travel, not a promised shipping schedule. Follow the repository for progress.

## Building from source

**Until KV4P-F-specific changes land, the build process is inherited from upstream KV4P-HT.**

When building from source, compile **both** the ESP32 firmware and Android app from the **same Git revision**. The firmware resource bundled with the Android app may be from the last public release. Source builds also lack a firmware compatibility check, so the phone and radio may appear to connect even when their versions are out of sync.

### Clone this fork

```bash
git clone https://github.com/Feedline-Labs/kv4p-ht.git
cd kv4p-ht
```

### ESP32 firmware

See [microcontroller-src/README.md](microcontroller-src/README.md). **PlatformIO** is the preferred development environment; Arduino IDE instructions are documented there as well.

Open `microcontroller-src/` as the PlatformIO project folder—it contains `platformio.ini`. **Do not** open the repository root as the PlatformIO project.

### Android app

Use **JDK 17**. Open `android-src/KV4PHT/` in Android Studio, or run:

```bash
cd android-src/KV4PHT
./gradlew assembleDebug
```

Debug APK: `android-src/KV4PHT/app/build/outputs/apk/debug/app-debug.apk`.

**Important:** A successful build from this early fork does **not** imply Greyline features are present. Do not confuse developer builds with a tested public KV4P-F release.

### Hardware and documentation

- Original project and hardware details: [kv4p.com](https://kv4p.com/)
- Firmware instructions: [microcontroller-src/README.md](microcontroller-src/README.md)
- PCB files: [`pcb/`](pcb/)
- 3D-printable cases: [`3d-print-case/`](3d-print-case/)
- Common problems: [FAQ.md](FAQ.md)

## Open source, credit, and independence

KV4P-F exists because the original KV4P-HT project made an innovative hardware-and-software radio platform available to the amateur radio community.

- **Original KV4P-HT project:** [VanceVagell/kv4p-ht](https://github.com/VanceVagell/kv4p-ht)
- **Original project website:** [kv4p.com](https://kv4p.com/)
- **Independent Feedline Labs fork:** [Feedline-Labs/kv4p-ht](https://github.com/Feedline-Labs/kv4p-ht)
- **Feedline Labs:** [feedlinelabs.com](https://feedlinelabs.com/)
- **Developer API:** [developer.feedlinelabs.com](https://developer.feedlinelabs.com/)

This repository retains the upstream project's **GNU General Public License v3.0 (GPL-3.0)**. See [LICENSE](LICENSE) and the upstream notices for applicable terms and attribution. Any Feedline Labs changes to GPL-covered code will remain subject to the GPL's requirements.

**KV4P-F is independently developed and maintained by Feedline Labs. It is not affiliated with, sponsored by, or endorsed by the original KV4P-HT project or its maintainers.**

## Get involved

Interested in the intersection of accessible ham radio hardware, live repeater activity, and genuinely useful connected features? Watch or star the [KV4P-F repository](https://github.com/Feedline-Labs/kv4p-ht), open an issue, or join the discussion as development progresses.

We're excited about what a radio can be when **offline reliability** and **online intelligence** reinforce each other.

**KV4P-F — Radio. Reimagined.**

*Off-grid by design. Connected by choice.*
