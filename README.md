# Pi-hole Block Lists

Block lists for Pi-hole targeting ads, telemetry, and tracking on home network devices.

## Main Blocklist

Blocks Roku ad servers, telemetry/logging endpoints, analytics platforms, and tracking infrastructure. Unnecessary for normal operation.

**File:** [Roku](Roku)

### Import into Pi-hole

**Option A — CLI (Pi-hole v6, recommended):**
```sh
pihole adlist add https://raw.githubusercontent.com/surfsire/pihole/refs/heads/main/Roku
pihole -g
```

**Option B — Adlist URL via Web UI (v5 and v6):**
1. Pi-hole Web UI → **Group Management → Adlists**
2. Paste the raw URL of the blocklist file
3. Run **Tools → Update Gravity** to apply

**Option C — Manual domain entry:**
1. Pi-hole Web UI → **Group Management → Domains**
2. Copy domains from the blocklist file directly

### Regex Filters

Optional but recommended. Regex filters catch subdomains not explicitly listed in the blocklist, providing broader coverage as Roku adds new endpoints over time. Note that this list may not be exhaustive.

Add separately under **Pi-hole → Domains → RegEx filter** (not hosts-format) or via CLI:

```sh
pihole --regex '(ads|logs)\.roku\.com$'
pihole --regex '(ads|logs|cloudservices)\.roku\.com$'
pihole --regex '(^|\.)ads\.roku\.com$'
pihole --regex '(^|\.)logs\.roku\.com$'
```

## Caution

Some domains may break functionality if blocked (e.g. firmware update servers).

If you experience issues after applying the blocklist, whitelist these domains first:

| Domain | Purpose |
|---|---|
| `roku.com` | Device activation |
| `my.roku.com` | Account management |
| `channelstore.roku.com` | Channel Store browsing |
| `update.roku.com` | Firmware updates |
| `rokutime.com` | Device clock sync (CDN) |
| `tvinteractive.tv` | Some channel interactive features |

**Web UI:**
- v5: **Group Management → Domains** → type: Whitelist
- v6: **Domains → Allow list**

**CLI:**
```sh
# Pi-hole v5
pihole -w roku.com
pihole -w my.roku.com
pihole -w channelstore.roku.com
pihole -w update.roku.com
pihole -w rokutime.com
pihole -w tvinteractive.tv

# Pi-hole v6
pihole allowlist add roku.com
pihole allowlist add my.roku.com
pihole allowlist add channelstore.roku.com
pihole allowlist add update.roku.com
pihole allowlist add rokutime.com
pihole allowlist add tvinteractive.tv
```

## DNS Bypass Warning

Roku may hardcode Google DNS (8.8.8.8 / 8.8.4.4) on newer firmware, bypassing Pi-hole entirely.

To force all DNS through Pi-hole, a firewall rule may be needed on your router redirecting port 53 traffic to your Pi-hole IP.

## Device Settings

Pi-hole blocking alone is not sufficient. Roku also has on-device ad and tracking settings that should be disabled manually.

### 1. Disable Advertising (standard Settings menu)

- **Settings → Privacy → Advertising**
  - Enable "Limit Ad Tracking"
  - Enable "Reset Advertising Identifier"

- **Settings → Privacy → Privacy Choices**
  - Check "Do Not Sell or Share My Personal Information"
  - Check "Limit Use of Sensitive Information"

### 2. Disable Home Screen Ads (Secret Screen 2)

Remote sequence: `Home×5, Up, Right, Down, Left, Up`

- **Cycle Scrollable Ads** → Always Disabled
- **Cycle Home Screen Ad Banner Server** → Demo 3
- Reboot device

> **Note:** Roku firmware updates may periodically reset these.

### 3. Disable Network Pings (Platform Secret Screen)

Remote sequence: `Home×5, FF, Play, Rewind, Play, FF`

- **System Operations Menu** → Disable Network Pings
- Press OK (option toggles to "Enable Network Pings")
- Press Home (do NOT press OK again)

> Stops constant router keep-alive pings that some routers misinterpret as attacks and block. Less privacy-relevant than ad/telemetry blocking but can help stability.

### 4. Disable Seasonal/Sponsored Wallpapers

- **Settings → Theme → Additional Settings** 
  - Turn Off Seasonal and Sponsored Ads

### 5. Hide Home Screen Recommendation Rows

- **Settings → Home screen → Recommendation Rows** → Hide
- **Settings → Home screen → Quick Access** → Hide
- **Settings → Home screen → Featured Free** → Hide
- **Settings → Home screen → Movie Store/TV Store** → Hide
- **Settings → Home screen → My Offers** → Hide

---

## Sources

Community research cross-referenced to compile this blocklist:

- [sidward35 - Adlist for Pi-hole with Roku, LG, and Samsung domains](https://gist.github.com/sidward35/cea28bedd0ec0b1bceec8c2b22c163c4) (GitHub Gist)
- [teawizardry - pihole-roku-list](https://github.com/teawizardry/pihole-roku-list) (GitHub)
- [jankytecj - roku-block-list](https://github.com/jankytech/roku-block-list) (GitHub)
- [Your Roku is tracking more than you think, here's how to limit it](https://www.howtogeek.com/your-roku-is-tracking-more-than-you-think-heres-how-to-limit-it/) How-To Geek Article By Rich Hein
- [How to Disable Ads on the Roku Home Screen](https://jasonpearce.com/2020/09/16/how-to-disable-ads-on-the-roku-home-screen/) Jason Pearce Blog
- [Pi-hole Community Discourse](https://discourse.pi-hole.net) (2025–2026)
- [Roku Forum - Remove Connect to Internet Requirement](https://community.roku.com/t5/Roku-Developer-Program/Request-Remove-Connect-to-Internet-Requirement/td-p/354035) (Defunct)

---

## Disclaimer

This blocklist is provided as-is for personal use. No guarantee is made that it is complete, accurate, or up to date. Blocking domains may break Roku features, apps, or services. Always test after applying and refer to the [Caution](#caution) section above. Not responsible for any loss of functionality, data, or service resulting from use of this list.

---

## License

This project is released into the public domain under [The Unlicense](LICENSE). Free to use, copy, modify, and distribute — no credit required.