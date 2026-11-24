# Feasibility: AI listing search and Polish-language AI calls to advertisers

**Date checked:** 2026-11-24 (every source below, unless stated otherwise).
**Scope:** a private individual in Poland who wants to **buy** a home (larger apartment, terraced, semi-detached or detached house). The tool is also a **portfolio project**. Nothing built yet.
**Updated 2026-11-24 with project decisions** from [decisions.md](../decisions.md). No new sources were read for the update. The research itself was done on the assumption of purely personal use.
**Reference only:** these findings are not input to the project's tasks unless the owner asks for them [D Q38].
**Not legal advice.** Points where a Polish lawyer's view would be decisive are marked **[LAWYER]**.

Labels: **[V]** verified, read in the primary source. **[I]** inference, my reasoning. **[U]** unverified, could not reach the primary source. **[D]** decision by the project owner, with its question number; not a research finding.

## Verdict per question

| Question | Verdict | Why |
|---|---|---|
| 1. Listing sources | **Feasible with conditions** | No sanctioned API was found that lets a private person search third-party resale or rental listings. Bulk scraping is barred or legally risky. Email alerts in your own inbox plus links you paste are the cleanest route found [I]. How to get listings is **not decided**: it is the separate Listing task, starting from the owner's own ideas [D Q24]. |
| 2. Telephony and voice | **Feasible with conditions** | ElevenLabs can place outbound calls over Twilio or a SIP trunk, Polish is supported, Claude can be the LLM, and a 4-minute call costs about USD 0.50-1.42. The weak point is the caller ID: Polish anti-spoofing rules can hide or block a +48 number on calls that enter Poland from abroad. |
| 3. Law | **Feasible with conditions** | Nothing found bans a genuine private enquiry by AI voice. The consent rule in PKE Art. 398 covers commercial information, which a private enquiry is not [I]. AI disclosure at the start is required by contract whatever the AI Act says. The GDPR household exemption is arguable, not safe, and the portfolio purpose weakens it further [I]. |

## What this means for the design

| Finding | Status |
|---|---|
| No sanctioned API found for third-party listings; some portals ban automated analysis | Listing is a separate task. Only real market listings are of interest [D Q24, Q26]. |
| Phone numbers are optional, may need login, and on one portal sit behind a paid service | Listing is a separate task [D Q24] |
| ElevenLabs' terms require a spoken notice at the start: AI, on whose behalf, why, which providers process the call, and recording if any [V] | AI, principal and purpose: **decided** [D Q6]. The provider notice still has to be written into the script. |
| Human approval fits low volume; Zadarma requires human supervision [V] | **Decided:** every call is approved by a person [D Q5] |
| ElevenLabs keeps conversations 2 years and saves audio by default [V] | **Decided:** no audio; transcript deleted after 30 days; facts kept with quotes [D Q18] |
| Opt-outs must be recognised and recorded [V] | **Decided:** block list [D Q6] |
| Caller ID may be hidden on calls from abroad | Phone number is a separate task [D Q31]. Real Twilio number [D Q21]. Family is called first [D Q4]; a caller-ID test is **not** a gate before real calls [D Q30]. |
| The repo is public: phone numbers, transcripts and listing copies must stay out of it. Publishing them would undermine the household exemption [I] | Transcripts of family calls are not published [D Q17] |

---

## 1. Listing sources

### Comparison

| Source | Sanctioned search/read access for a private person | Phone number | Terms | robots.txt |
|---|---|---|---|---|
| **Otodom** | None found. The [partner API](https://developer.olxgroup.com/reference) creates, updates and deletes adverts; the [XML feed](https://developer.olxgroup.com/docs/getting-started) publishes an agency's own adverts [V]. | Shown only "jeśli został podany podczas publikacji Ogłoszenia" (§4.2(a)) [V, old text] | §2.3, see below | One group for all bots; `Disallow: /api/query`, `Allow: /`, and [`Content-Signal: search=yes, ai-input=yes, ai-train=no`](https://www.otodom.pl/robots.txt) [V] |
| **OLX.pl** | None found. The [developer page](https://developer.olx.pl/api/doc) is addressed to partners acting "on behalf of sellers" [V]; full endpoint list [U]. | Optional; "Użytkownik może zostać poproszony o zalogowanie się na Konto" to see it (§4.2) [V] | §2.3, see below | [Blocks `/api/`, `*/rss/` and `?q=` searches, but allows `/api/v1/offers/`](https://www.olx.pl/robots.txt) [V]. A robots allowance is not a licence [I]. |
| **Nieruchomosci-online** | None found | Advertiser must give working contact details (§13(1)(5)); click/login [U] | §14a, the clearest ban, see below | [Rules for named bots only; no `*` group](https://www.nieruchomosci-online.pl/robots.txt) [V] |
| **Domiporta** | None found | Contact form; click/login [U] | Art. 3(15), see below | [Blocks Scrapy entirely; path rules for others](https://www.domiporta.pl/robots.txt) [V] |
| **Adresowo** | None found | Contact with direct sellers on free ads requires paid access (§6(21)) [V] | §3(9) bans parsing "przy użyciu botów" | [Path rules only](https://adresowo.pl/robots.txt) [V] |
| **Allegro Lokalnie** | None. Allegro staff, 26 Oct 2026: "Zasób /offers/listing nie jest już dostępny w publicznym REST API Allegro" ([issue 14213](https://github.com/allegro/allegro-api/issues/14213)) [V] | [U] | [U] allegro.pl refused the fetch (403) | [Only `/oferty/wystaw/*` blocked](https://allegrolokalnie.pl/robots.txt) [V] |
| **Facebook Marketplace** | Not for individuals. Meta Content Library has a [Marketplace guide](https://developers.facebook.com/docs/content-library-and-api/content-library-api/guides/fb-marketplace) but access appears limited to researchers at qualifying institutions [U, page body not readable] | [U] | Terms §3.2(3) | [Blocks `*` and ClaudeBot from the whole site](https://www.facebook.com/robots.txt) [V] |
| **Gratka, Morizon** | [U] both sites could not be fetched | [U] | [U] | [U] |
| **dane.gov.pl developer prices** (**rejected** [D Q24, Q26]: not real market listings) | **Yes, open data.** Developers must send price data daily; the minister publishes it as high-value data (Act of 20 May 2021, [Dz.U. 2026 poz. 880](https://api.sejm.gov.pl/eli/acts/DU/2026/880/text.pdf), Art. 19a-19b) [V]. A uniform data structure was mandated with effect 3 months after 10 Aug 2026 ([poz. 1077](https://api.sejm.gov.pl/eli/acts/DU/2026/1077/text.pdf)) [V]. A sample dataset is [CC0, daily](https://api.dane.gov.pl/1.4/datasets/19458) [V]. | Developer's sales contact, not private people | Open licence | n/a |
| **RCN (transaction prices)** (not decided) | **Yes, free** since the Act of 26 Sept 2025: [WMS/WFS since Feb 2026, GeoParquet and GeoPackage downloads](https://www.geoportal.gov.pl/pl/dane/rejestr-cen-nieruchomosci-rcn/) [V]. Past sales, not listings. | None | Open | n/a |
| **Commercial data** | Otodom Analytics sells monthly market statistics, not a listing feed with contacts [U, search result only]. I found no sanctioned commercial feed of third-party listings sold to individuals. Apify-style "Otodom scraper" products are unofficial. | n/a | n/a | n/a |

### The decisive clauses

- **OLX §2.3** (footer reads, garbled, "Obowiązuje od 13 marca lutego 2026 r."; the newest version I found, [PDF](https://pomoc.olx.pl/olxplhelp/sfc/servlet.shepherd/document/download/069Wy00000MFKzdIAH)) [V]: "Zabrania się jakiegokolwiek wykorzystywania tych treści bez pisemnej zgody uprawnionych. Zabrania się jakiegokolwiek agregowania i przetwarzania danych oraz innych informacji dostępnych w Serwisie **w celu ich dalszego udostępniania osobom trzecim**…" (No use of content without written consent; no aggregating or processing of data *in order to pass it on to third parties*.) The aggregation ban is limited by purpose; the content-use ban is not.
- **Otodom §2.3**: the same wording in the copy served to me, dated 17 Feb 2024 ([regulamin](https://www.otodom.pl/regulamin)) [V for that copy]. Search results show a newer version, in force from 29 July 2026 ([help page](https://pomoc.otodom.pl/otodomhelpcenter/s/article/regulamin-dla-u%C5%BCytkownikw-indywidualnych-V17-odo)), whose text I could not read [U]. A human should read it.
- **Nieruchomosci-online §14a(1)** ([regulamin](https://www.nieruchomosci-online.pl/regulamin.html)) [V]: "zabrania się jakiegokolwiek automatycznego pobierania, kopiowania, indeksowania lub analizowania danych zamieszczonych w Portalu, w tym przy użyciu programów, skryptów, botów, crawlerów…" (any automatic downloading, copying, indexing or *analysing* is banned). §14a(2) preserves use of an insubstantial part of the database for any purpose.
- **Domiporta Art. 3(15)** ([regulamin](https://www.domiporta.pl/regulamin-serwisu)) [V]: "Wykorzystanie jakichkolwiek materiałów zgromadzonych na Serwisie…" needs the owner's prior consent. No clause on bots.
- **Adresowo §3(9)** ([regulamin](https://adresowo.pl/info/terms.html)) [V]: bans access "przez parsowanie Serwisu przy użyciu botów"; §3(10) restricts VPNs and proxies.
- **Meta Terms §3.2(3)** ([terms](https://www.facebook.com/legal/terms)) [V]: "Użytkownik nie może uzyskiwać dostępu ani gromadzić danych z naszych Produktów sposobem zautomatyzowanym" without prior consent.

None of the clauses I read deals with one person phoning an advertiser about their own ad. Adresowo §7(5) bars *advertising* messages to advertisers' contact details [V]; an enquiry is not advertising [I].

### Sui generis database right

Polish Act of 27 July 2001, consolidated [Dz.U. 2024 poz. 1769](https://api.sejm.gov.pl/eli/acts/DU/2024/1769/text.html) [V] (implements Directive 96/9/EC):

- Art. 6(1): the producer has the exclusive right to extract and re-use the whole or a substantial part.
- Art. 7(1), (4): a lawful user may extract an **insubstantial** part for any purpose, and contract terms to the contrary are **void**. Art. 7(3): this must not harm normal use or the producer's legitimate interests.
- Art. 8(1)(1): the personal-use exception covers only **non-electronic** databases. It does not help here.
- Art. 8(2): repeated and systematic extraction conflicting with normal use is not allowed.
- Art. 8a: text and data mining is allowed unless the rightsholder has reserved it in machine-readable form. Otodom's `ai-input=yes` signal is relevant, but whether ad hoc listing lookup counts as TDM, and whether prose terms count as a reservation, is **[LAWYER]**.

**[I]** Reading a handful of listings sits within Art. 7. A scheduled crawler across a portal drifts towards Art. 6 and 8(2), as well as the terms. Blocking also matters in practice: Otodom refused my plain command-line client (403) while serving the WebFetch tool. I did not retry the blocked client, but I did re-read robots.txt with WebFetch.

### Compliant fallbacks

Research options only; none is chosen. Listing is a separate task [D Q24].

| Route | Assessment |
|---|---|
| **Saved-search alerts parsed from your own inbox** | Best fit. Otodom §5.8-5.9 (2024 copy): instant alerts at least 5 minutes after publication, at most 25 a day, and may be disabled after 60 days without opening one [V, old text]. Domiporta Art. 12(2): free email alerts ("Powiadamiacz") [V]. OLX's terms mention push alerts for watched ads [V]; email alerts [U]. I found no clause forbidding you to process emails sent to you; what matters is not republishing [I]. |
| **User-pasted links or text** | Low risk on a single listing [I]. Prefer pasted text or a screenshot over an automated fetch on sites that ban automated analysis (Nieruchomosci-online) or parsing (Adresowo). |
| **RSS** | OLX disallows `*/rss/` in robots.txt [V]. No official RSS found elsewhere. |
| **Open data (dane.gov.pl, RCN)** | Licence allows use. dane.gov.pl is **rejected** [D]; using RCN as a price benchmark is not decided and belongs to the Score task [D Q25]. |
| **Written permission from one portal** | Possible, outcome unknown. |

---

## 2. Telephony and voice

### Telephony routes

**Twilio with a real number is chosen** [D Q21]. The other routes stay here as alternatives. Number type and set-up are open: the Phone number task [D Q31].

| Route | Works with ElevenLabs | Polish number for a private person | Call to PL mobile | Main risk |
|---|---|---|---|---|
| **Twilio native** | Yes: [purchased numbers in/out; verified caller IDs outbound only](https://elevenlabs.io/docs/eleven-agents/phone-numbers/twilio-integration/native-integration) [V] | [ID, proof of PESEL or other ID number, address](https://www.twilio.com/en-us/guidelines/pl/regulatory). Local number: address in that area. Mobile (+4873, +4879): address anywhere [V] | [USD 0.0715/min "from EEA", otherwise 0.2202](https://www.twilio.com/en-us/voice/pricing/pl); local number 2.90/month [V]. Mobile rental not published [U] | [Outbound caller ID not guaranteed; non-Twilio verified caller ID "best effort"](https://www.twilio.com/en-us/guidelines/pl/voice) [V] |
| **Twilio Elastic SIP** | Yes, via [SIP trunk](https://elevenlabs.io/docs/eleven-agents/phone-numbers/sip-trunking) [V] | Same | [USD 0.0675/min from EEA, otherwise 0.2162](https://www.twilio.com/en-us/sip-trunking/pricing/pl) [V] | Same |
| **Telnyx** | Yes, [documented](https://elevenlabs.io/docs/eleven-agents/phone-numbers/telephony/telnyx) [V] | [ID copy, address proof under 3 months; mobile needs an address in Poland](https://support.telnyx.com/en/articles/5466967-poland-did-requirements) [V] | PL mobile rate not found [U]; local numbers [from USD 1/month](https://telnyx.com/phone-numbers/poland.md) [V] | CLI handling [U] |
| **Zadarma** | Yes: [its guide](https://zadarma.com/en/support/instructions/elevenlabs/) covers ElevenLabs SIP including outbound [V] | [Mobile](https://zadarma.com/en/tariffs/numbers/poland/mobile/): ID and Polish address, PLN 10/month + 20 setup. [Landline](https://zadarma.com/en/tariffs/numbers/poland/plock): PLN 7/month, address in the region [V] | [PLN 0.084/min (0.59 with a non-EU caller ID)](https://zadarma.com/tariffs/calls/poland/), net of VAT. Table labels are ambiguous; low = EU caller ID is my reading [I] | Policy: ["strictly prohibits robocalling"; "All calls must be made under human supervision and interaction"](https://zadarma.com/en/blog/call-center-automation/) [V] |
| **Polish operator SIP trunk** | Technically yes [I] | Varies [U] | [U] | Least exposed to the anti-spoofing rules, if the call originates inside Poland on that operator's own number [I] |

Zadarma's own AI-agent product adds: ["Calls may only be made to customers who have agreed to receive them"](https://zadarma.com/en/services/ai-voice-agent/) [V]. Whether that binds a third-party agent over SIP is not stated. Ask before buying.

### Caller ID and Polish anti-spoofing rules

Act of 28 July 2023 on combating abuse in electronic communications ([consolidated text](https://api.sejm.gov.pl/eli/acts/DU/2023/1703/text/U/D20231703Lj.pdf)) [V]:

- Art. 3(1)(3): CLI spoofing means *unauthorised* use of a number pointing to another person *to impersonate them*. Showing your own number is not spoofing [I].
- Art. 16: operators must nevertheless **block the call or hide the number** to fight spoofing.
- Art. 31: criminal liability (3 months to 5 years) for impersonation by caller ID to gain, among other things, personal data.

[UKE recommendations, version 2, 17 Sept 2026](https://bip.uke.gov.pl/download/gfx/bip/pl/defaultaktualnosci/49/28/1/rekomendacje_prezesa_uke_cli_spoofing_wer2.pdf) (for operators outside the big-operator agreement) [V]:

- **A.1(1)(3)**: a user may originate calls only from a number assigned in a contract with *that* provider. Read strictly, this defeats using your existing personal mobile number as caller ID through Twilio [I].
- **A.2**: a call using Polish numbering may not be initiated outside Poland, except roaming or when traffic does not arrive over international links.
- **B.2**: operators should **hide the number** when a Polish **landline** number arrives over an international link. No equivalent line for mobile numbers.

The four largest operators follow a separate agreement: suspect calls are [rejected or connected without a number](https://uke.gov.pl/blog/operatorzy-wdroza-skuteczne-rozwiazania-przeciwdzialajace-podszywaniu-sie-pod-numery-telefonow-podpisalismy-porozumienie,109.html), and UKE says the technical details will not be published [V]. So only a live test will tell. **[I]** Expect a cloud-provider +48 landline number to risk appearing as "numer zastrzeżony", which many sellers will not answer. A mobile number, or a Polish operator, is the better first test. Not decided; Phone number is a separate task [D Q31].

### Polish voice, models, features

- **TTS**: Flash v2.5 (about 100 ms median, 32 languages including Polish) and v4 Turbo (about 100 ms, 90+ languages including Polish), excluding network and application time ([models](https://elevenlabs.io/docs/overview/models)) [V].
- **STT**: Polish is in the best band for Scribe v2, "Excellent (≤ 5% WER)" ([docs](https://elevenlabs.io/docs/overview/capabilities/speech-to-text)) [V]. That is not stated to be telephone audio. I found no Polish-specific TTS quality data.
- **Realtime models**: average time to first audio 620 ms (Mini) and 570 ms (Max) on a benchmark; the [page](https://elevenlabs.io/realtime) does not name Polish [V].
- **LLM** (model not decided): Claude is native: Opus 5.5 to 4.6, Sonnet 5.5 to 4.5, Haiku 4.5, alongside GPT, Gemini and others, or a custom LLM ([docs](https://elevenlabs.io/docs/eleven-agents/customization/llm)) [V]. Haiku 5.5 is not listed there. No per-language LLM statement found.
- **Voicemail detection**: built in; leave a message or hang up ([docs](https://elevenlabs.io/docs/eleven-agents/customization/tools/system-tools/voicemail-detection)) [V]. Hang up, to limit data left with third parties [I].
- **Transfer to a human**: cold or warm on Twilio; conference or SIP REFER on SIP ([docs](https://elevenlabs.io/docs/eleven-agents/customization/tools/system-tools/transfer-to-number)) [V].
- **Retention**: default 2 years; set any number of days, 0 schedules deletion ([docs](https://elevenlabs.io/docs/eleven-agents/customization/privacy/retention)). Audio saving is on by default and [can be disabled](https://elevenlabs.io/docs/eleven-agents/customization/privacy/audio-saving). [Zero Retention Mode](https://elevenlabs.io/docs/eleven-agents/customization/privacy/zrm) can be set per agent; results then arrive only by webhook [V]. Which plans include it [U].
- **Webhooks**: [post-call transcript and audio, HMAC-signed](https://elevenlabs.io/docs/eleven-agents/workflows/post-call-webhooks) [V].
- **Batch calling**: exists but [is being replaced by Campaigns and cannot use ZRM](https://elevenlabs.io/docs/eleven-agents/phone-numbers/batch-calls) [V]. Not needed.
- **EU data residency**: [Enterprise only](https://elevenlabs.io/docs/overview/administration/data-residency); standard storage is in the US [V].

### Cost of one 4-minute call to a Polish mobile

Net of VAT. Prices checked 2026-11-24; [NBP](https://api.nbp.pl/api/exchangerates/rates/a/usd/?format=json) USD 1 = PLN 4.0601 (23 Nov 2026).

| Item | Calculation | USD |
|---|---|---|
| ElevenLabs agent ([rate](https://elevenlabs.io/pricing/agents)) | 4 × 0.08 | 0.320 |
| LLM, Claude Haiku 4.5 | 20 turns × 4,000 input = 80k × $1/M = 0.080; 20 × 100 output = 2k × $5/M = 0.010; × 1.1 | 0.099 |
| LLM, Claude Sonnet 5.5 | (80k × $2/M + 2k × $10/M) × 1.1 | 0.198 |
| Twilio to PL mobile, EEA rate / non-EEA rate | 4 × 0.0715 / 4 × 0.2202 | 0.286 / 0.881 |
| Twilio Media Streams (assumed to apply [I]) | 4 × 0.0044 | 0.018 |
| Zadarma, EU caller ID / other | 4 × PLN 0.084 / 4 × PLN 0.59, divided by 4.0601 | 0.083 / 0.581 |

| Scenario | Total |
|---|---|
| Zadarma EU + Haiku 4.5 | 0.320 + 0.099 + 0.083 = **USD 0.50 (PLN 2.04)** |
| Twilio EEA + Haiku 4.5 | 0.320 + 0.099 + 0.286 + 0.018 = **USD 0.72 (PLN 2.93)** |
| Twilio non-EEA + Sonnet 5.5 (worst case) | 0.320 + 0.198 + 0.881 + 0.018 = **USD 1.42 (PLN 5.75)** |

Token counts are my assumption, with no caching; [Claude prices](https://platform.claude.com/docs/en/about-claude/pricing) are published, and ElevenLabs passes LLM cost through, with 10% added on "select" Claude models. Sonnet 5.5's newer tokenizer yields more tokens, so treat its line as a floor. Twilio's EEA rate depends on an EEA caller ID [U, help page body not readable].

**Fixed monthly:** ElevenLabs Starter USD 6 (75 minutes included, about 18 such calls) + Twilio local number USD 2.90 = **USD 8.90 (PLN 36.13)**. With Zadarma: USD 6 + PLN 10 mobile = **about PLN 34** plus PLN 20 once. Both fit the limit of PLN 200 a month [D Q10], but evaluation runs are not included in these figures.

### Provider rules on AI calls

- **ElevenLabs Agents Terms, 9 Oct 2026** ([text](https://elevenlabs.io/agents-terms)) [V]. §3.5: before interaction, tell the person clearly they are talking to AI, that ElevenLabs and the LLM provider process the call, and that it is recorded if so. §3.6, outbound: obtain all consents and give all disclosures required by applicable telecom law **before** the call; state at the start **on whose behalf** and why; offer and record opt-outs. §5: you indemnify ElevenLabs. The terms speak of a "business or entity"; they do not address personal use.
- **Use policy** ([text](https://elevenlabs.io/use-policy)) [V]: no calls that are unlawful or lack legally required consent; no cloning a voice without consent.
- **EU consumer terms** ([text](https://elevenlabs.io/terms-of-use-eu)) [V]: content may be used to improve models unless you opt out under "Data use". Opt out before the first call. The free plan is non-commercial only.
- **Anthropic usage policy, 12 Nov 2026** ([text](https://www.anthropic.com/legal/aup)) [V]: agents talking to external users must disclose they are AI at the start. LLM providers' terms bind you directly (ElevenLabs §1.2).
- **Twilio**: the [Voice policy](https://www.twilio.com/en-us/legal/service-country-specific-terms/voice-sip) requires prior consent where law requires it for telemarketing, and bans misleading caller ID; AI is not addressed. The [AUP](https://www.twilio.com/en-us/legal/aup) bans misleading people about the sender [V].

---

## 3. Law

### GDPR

**Household exemption.** [Regulation 2016/679](http://publications.europa.eu/resource/celex/32016R0679) Art. 2(2)(c) excludes a natural person's "purely personal or household activity"; recital 18 adds "no connection to a professional or commercial activity" and that the Regulation still applies to those who **provide the means** [V].

- For: finding your own home is personal; *Ryneš* treats correspondence and address books as personal "even if they incidentally concern… the private life of other persons" ([C-212/13](http://publications.europa.eu/resource/celex/62013CJ0212), para 32) [V]. EDPB Statement on smart glasses (13 Oct 2026, [PDF](https://www.edpb.europa.eu/system/files/2026-10/20261013_edpb_statement_smart-glasses_en.pdf), paras 13-17) accepts a user relying on it even when an AI provider processes the data, while the provider may itself be a controller [V].
- Against: narrow construction, "purely" (*Ryneš* paras 29-30); activity "directed outwards from the private setting" falls outside (*Ryneš* para 33; [C-25/17](http://publications.europa.eu/resource/celex/62017CJ0025) para 44). EDPB [Guidelines 3/2019](https://www.edpb.europa.eu/sites/default/files/files/file1/edpb_guidelines_201903_video_devices_en_0.pdf) para 13 weigh: no personal relationship with the people, scale or frequency suggesting professional activity, adverse impact [V].
- **[I]** A few approved calls for your own home: plausibly exempt. Dozens of automated calls to strangers, recorded and stored in the US: weaker. Buy-to-let, helping others, or publishing results: assume not exempt. This project is a portfolio piece [D Q1], other people will test it [D Q3, Q20] and real advertisers are to be called [D Q4], so it sits closer to the non-exempt side [I]. **[LAWYER]** I found no CJEU, EDPB or UODO ruling on AI calls by private people. The exemption never covers ElevenLabs, Twilio or Anthropic.

**If not exempt, design for this anyway (it is cheap):**

| Duty | Position |
|---|---|
| Lawful basis | Art. 6(1)(f). [EDPB Guidelines 1/2024 v2.0](https://www.edpb.europa.eu/system/files/2026-11/edpb_guidelines_202401_legitimateinterest_v2_en.pdf) (10 Nov 2026): documented three-step test; public availability is only a factor (fn. 65); reasonable expectations count (paras 47-51) [V]. An advertiser expects enquiry calls, but not necessarily an AI recording them [I]. |
| Information | Art. 14(3)(b): at the latest **at first communication**; Art. 13 for what they say on the call. Short spoken notice plus a fuller one on request [I]. |
| Voice | EDPB Guidelines 02/2021 para 31: voice data is "inherently biometric personal data"; Art. 9 applies only if used to uniquely identify a person ([PDF](https://www.edpb.europa.eu/system/files/2021-07/edpb_guidelines_202102_on_vva_v2.0_adopted_en.pdf)) [V]. Use no voice identification. |
| Processors | Art. 28(3) contract. The [ElevenLabs DPA](https://elevenlabs.io/dpa) covers self-serve customers [V], but the consumer terms mention it for entities; confirm it covers an individual. Twilio has a [DPA](https://www.twilio.com/en-us/legal/data-protection-addendum) [V]. |
| Transfers | ElevenLabs: DPF or SCCs. Twilio: DPF, BCRs, SCCs [V]. General Court upheld the DPF on 3 Sept 2025 ([T-553/23](http://publications.europa.eu/resource/celex/62023TJ0553)) [V]; appeal C-703/25 P reported pending [U]. |
| Retention, rights | Transcript deleted after 30 days [D Q18]; how long facts are kept is not decided; support access, erasure and objection (Art. 21(1)). |
| Profiling people | Extract facts about the flat, not about the person [I]. |

### Polish telecom law (PKE)

[Act of 12 July 2024](https://api.sejm.gov.pl/eli/acts/DU/2024/1221/text/U/D20241221Lj.pdf), text as of 16 Oct 2026, **Art. 398(1)** [V]: "Zakazane jest używanie: 1) automatycznych systemów wywołujących, 2) telekomunikacyjnych urządzeń końcowych… do celów przesyłania **informacji handlowej**… w tym marketingu bezpośredniego… chyba że uprzednio wyraził on na to zgodę." (Automatic calling systems and terminal devices are banned *for sending commercial information* without prior consent.)

"Commercial information" is defined in the [e-services Act](https://api.sejm.gov.pl/eli/acts/DU/2002/1204/text/U/D20021204Lj.pdf) Art. 2(2) [V]: information intended to **promote goods, services or the image of an entrepreneur** or regulated professional.

- **[I]** A private buyer or tenant asking about a flat promotes nothing, so Art. 398 should not apply, even if the dialler is an "automatic calling system". Art. 398 restricts the purpose, not the technology. **[LAWYER]** if calls ever offer a service.
- Where it applies, consent is needed from businesses too, and calling to *ask for* consent is not allowed ([minister's reply citing UKE/UOKiK/UODO](https://api.sejm.gov.pl/sejm/term10/interpellations/attachment/ATTDMGHNP/i12337-o1.pdf)) [V]. Penalty: up to 3% of revenue or PLN 1,000,000, whichever is higher (Art. 446(5)) [V].
- Stay clear of the line: no offers on behalf of others, no estate-agency use, no selling of results. An agent flat-hunting for clients would be commercial.
- No UKE or UODO statement on AI voicebots under Art. 398 was found.

### Recording

- **Penal Code Art. 267 §3** ([text](https://api.sejm.gov.pl/eli/acts/DU/1997/553/text/U/D19970553Lj.pdf)) [V]: a crime to use a listening "or other device **or software**" to obtain information one is **not entitled to**; prosecuted on the victim's motion (§5).
- The Supreme Court ([III KK 265/15](https://www.saos.org.pl/api/judgments/245211), 27 Apr 2016) held that §3 protects statements the participants, at least implicitly, made confidential, that any recorder counts as a device, and called "erroneous" the view that a dictaphone is not a listening device when recording someone unaware [V]. That case concerned left or hidden recorders. The common view that a participant may record their own call rests on rulings I could not open (II CSK 478/15, I CSK 292/17; sn.pl redirect loop) [U].
- **Is an AI agent a "participant"?** No authority found **[LAWYER]**. **[I]** The doubt largely disappears if the person is told at the start and continues. Covert recording by an AI while the user is absent is the riskiest design; do not build it.
- **Civil Code Arts. 23-24** ([text](https://api.sejm.gov.pl/eli/acts/DU/1964/93/text/U/D19640093Lj.pdf)) [V]: an open list of personal interests including secrecy of correspondence; remedies include cessation, compensation. Announced, minimal, unpublished recording greatly lowers the risk [I].
- No audio is kept [D Q18]. A transcript is still a record. Turning off audio does not remove the duty to inform.

### EU AI Act, Article 50

**Status.** [Regulation 2024/1689](http://publications.europa.eu/resource/celex/32024R1689) Art. 113: general application 2 Aug 2026, which includes Art. 50 [V]. The Digital Omnibus on AI **was adopted**: [Regulation 2026/1744](http://publications.europa.eu/resource/celex/32026R1744), OJ 24 July 2026, in force on the third day after publication [V]. It replaces Art. 50(7) (codes of practice) and adds Art. 111(4): only providers of generative systems already on the market before 2 Aug 2026 get until **2 Dec 2026**, and only for Art. 50(2) machine-readable marking [V]. **Art. 50(1) and 50(4) were not postponed and apply now.**

**Guidelines.** Commission guidelines [C(2026) 7702 final, 5 Nov 2026](https://ec.europa.eu/newsroom/dae/redirection/document/131215) [V]:

- Para 30: interaction includes **phone** and need not be started by the human.
- Para 31: AI agents dealing with people (bookings, negotiating) must disclose **both** that they are AI **and the person on whose behalf they act**.
- Para 37: in telephony, an explicit spoken statement at the beginning; tones alone are not enough.
- Para 45: "obvious" only where there is almost no doubt. A human-like voice on a cold call will not qualify [I].
- Para 30(iii): human ability to intervene does not remove the duty.

A voluntary [Code of Practice](https://digital-strategy.ec.europa.eu/en/news/commission-publishes-code-practice-marking-and-labelling-ai-generated-content) on marking and labelling was published in June 2026 [U, search result]; it mainly concerns generated content, not calls.

**Does it bind a private person?**

| Duty | On whom | Private user |
|---|---|---|
| 50(1) tell people it is AI | **Provider** | The personal-activity exclusion (Art. 2(10), Art. 3(4)) covers **deployers only** (guidelines para 20) [V]. "Provider" includes putting a system into service under one's own name, and "putting into service" includes **own use** (Art. 3(3), 3(11)) [V]. The guidelines' own-use example is an organisation (para 11). ElevenLabs is the natural provider, but someone who configures an agent could be argued to be one **[LAWYER]**. |
| 50(2) machine-readable marking | Provider | ElevenLabs' task [I] |
| 50(4) deepfake label | Deployer | Excluded for purely personal, non-professional use (para 19) [V]. Regular economic benefit ends the exclusion. |

**Design answer:** always disclose, so the open question has little practical effect. **Decided** [D Q6].

**Poland.** Act of 3 July 2026 on AI systems, [Dz.U. 2026 poz. 1003](https://api.sejm.gov.pl/eli/acts/DU/2026/1003/text.pdf), in force 11 Aug 2026, with chapters 3-5, 8 and 9 and Arts. 8-18 three months after publication on 27 July 2026. Art. 3(1)(1) excludes individuals **as deployers** using AI only for personal purposes [V]. It again says nothing about individuals as providers.

### Agencies, entrepreneurs and "do not call again"

- A business line reduces privacy risk, but a named agent's voice and number are still personal data [I], and the AI disclosure applies when a person represents a company (guidelines para 31) [V].
- "Do not call again": ElevenLabs §3.6(c) makes it binding on you [V]; GDPR Art. 21 if GDPR applies; persistent calling could become malicious harassment ([Petty Offences Code](https://api.sejm.gov.pl/eli/acts/DU/1971/114/text/U/D19710114Lj.pdf) Art. 107: "złośliwie niepokoi") [V text; application I]. Block the number permanently, across all listings. Poland has no national do-not-call register that I found.

**Buying or renting makes no legal difference that I found** [I], except that buy-to-let edges towards economic activity. The project is about buying a home to live in [D Q2, Q4].

---

## Open questions

**Answered by the project owner**
1. Buying, for their own home [D Q2, Q4].
2. Per-call approval: yes [D Q5].
3. Audio recording: no; transcript for 30 days [D Q18].
4. Name given by the agent: the real name on real calls, a fictional one in the demo [D Q6].

**Topics that belong to separate tasks (for reference)**
5. **Listing** [D Q24]: sources and portals; current Otodom terms (29 July 2026) at [pomoc.otodom.pl](https://pomoc.otodom.pl/otodomhelpcenter/s/article/regulamin-dla-u%C5%BCytkownikw-indywidualnych-V17-odo); Gratka and Morizon terms and robots.txt; [Allegro terms](https://allegro.pl/regulamin); a newer OLX version, if any; where the phone number comes from; database right and TDM if any automated fetching is planned **[LAWYER]**.
6. **Score** [D Q25]: what "attractive price" is compared with, and how recent a listing must be.
7. **Phone number** [D Q31]: number type, documents, Twilio mobile price and EEA-rate conditions; how the number shows on Orange, Play, Plus and T-Mobile; other providers only if Twilio does not work.
8. **First conversation** (POC): quality of Polish over the phone, including addresses, prices and numerals.

**Open, no task yet**
9. ElevenLabs: does the DPA cover an individual, and is ZRM available on Starter? Opt out of training before the first call.
10. For a Polish lawyer: household exemption with US processors, given the portfolio purpose; Art. 267 §3 and "participant" status when an AI takes part and the user does not; whether the user is an AI Act "provider" by own use. The owner chose **no extra gates** before the first real call [D Q30], and **no legal review** before it [D], so these questions stay open.

---

## What I could not verify

- **Refused or unreadable:** gratka.pl and morizon.pl (tool could not fetch); allegro.pl (403); otodom.pl and developer.olx.pl to a command-line client (403; pages quoted were served to WebFetch); ElevenLabs, Twilio and Otodom help centres (403 or empty); Meta Content Library eligibility (empty body); sn.pl (redirect loop); uodo.gov.pl (certificate error); InfoCuria (empty). EUR-Lex pages came back empty, so EU texts were read from the Publications Office Cellar service.
- **Not found:** a UODO or UKE position on AI voicebots or private AI callers; the text of the big-operator CLI agreement (UKE says technical details are not public); Telnyx's Polish mobile rate; Polish TTS quality data.
- No listing pages were opened, so click or login gates on phone numbers come from terms only.

## Main sources

**EU law:** [GDPR](http://publications.europa.eu/resource/celex/32016R0679) · [AI Act](http://publications.europa.eu/resource/celex/32024R1689) · [Reg. 2026/1744](http://publications.europa.eu/resource/celex/32026R1744) · [C-212/13](http://publications.europa.eu/resource/celex/62013CJ0212) · [C-25/17](http://publications.europa.eu/resource/celex/62017CJ0025) · [T-553/23](http://publications.europa.eu/resource/celex/62023TJ0553) · [Commission Art. 50 guidelines](https://ec.europa.eu/newsroom/dae/redirection/document/131215) · [EDPB 1/2024 v2](https://www.edpb.europa.eu/system/files/2026-11/edpb_guidelines_202401_legitimateinterest_v2_en.pdf) · [EDPB 3/2019](https://www.edpb.europa.eu/sites/default/files/files/file1/edpb_guidelines_201903_video_devices_en_0.pdf) · [EDPB 02/2021](https://www.edpb.europa.eu/system/files/2021-07/edpb_guidelines_202102_on_vva_v2.0_adopted_en.pdf) · [EDPB smart glasses](https://www.edpb.europa.eu/system/files/2026-10/20261013_edpb_statement_smart-glasses_en.pdf)

**Polish law:** [PKE](https://api.sejm.gov.pl/eli/acts/DU/2024/1221/text/U/D20241221Lj.pdf) · [Anti-abuse Act](https://api.sejm.gov.pl/eli/acts/DU/2023/1703/text/U/D20231703Lj.pdf) · [e-services Act](https://api.sejm.gov.pl/eli/acts/DU/2002/1204/text/U/D20021204Lj.pdf) · [Database Act](https://api.sejm.gov.pl/eli/acts/DU/2024/1769/text.html) · [Penal Code](https://api.sejm.gov.pl/eli/acts/DU/1997/553/text/U/D19970553Lj.pdf) · [Civil Code](https://api.sejm.gov.pl/eli/acts/DU/1964/93/text/U/D19640093Lj.pdf) · [AI systems Act](https://api.sejm.gov.pl/eli/acts/DU/2026/1003/text.pdf) · [Developer Act](https://api.sejm.gov.pl/eli/acts/DU/2026/880/text.pdf) · [UKE recommendations v2](https://bip.uke.gov.pl/download/gfx/bip/pl/defaultaktualnosci/49/28/1/rekomendacje_prezesa_uke_cli_spoofing_wer2.pdf) · [III KK 265/15](https://www.saos.org.pl/api/judgments/245211)

**Providers:** [ElevenLabs Agents Terms](https://elevenlabs.io/agents-terms) · [pricing](https://elevenlabs.io/pricing/agents) · [disclosure docs](https://elevenlabs.io/docs/eleven-agents/legal/disclosure-requirement) · [Twilio PL voice](https://www.twilio.com/en-us/guidelines/pl/voice) · [Twilio PL pricing](https://www.twilio.com/en-us/voice/pricing/pl) · [Zadarma](https://zadarma.com/en/support/instructions/elevenlabs/) · [Telnyx PL](https://support.telnyx.com/en/articles/5466967-poland-did-requirements) · [Anthropic AUP](https://www.anthropic.com/legal/aup)

**Listing sources:** linked in section 1.
