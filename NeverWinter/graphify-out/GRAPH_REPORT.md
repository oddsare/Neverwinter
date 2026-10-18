# Graph Report - .  (2026-10-17)

## Corpus Check
- Large corpus: 74 files · ~883,924 words. Semantic extraction will be expensive (many Claude tokens). Consider running on a subfolder, or use --no-semantic to run AST-only.

## Summary
- 879 nodes · 1583 edges · 50 communities detected
- Extraction: 91% EXTRACTED · 7% INFERRED · 2% AMBIGUOUS · INFERRED: 115 edges (avg confidence: 0.74)
- Token cost: 340,550 input · 0 output

## Community Hubs (Navigation)
- [[_COMMUNITY_Neverwinter Core & Renaer|Neverwinter Core & Renaer]]
- [[_COMMUNITY_Dragons & Cult of the Dragon|Dragons & Cult of the Dragon]]
- [[_COMMUNITY_Lords' Alliance & Northern Cities|Lords' Alliance & Northern Cities]]
- [[_COMMUNITY_Waterdeep, Manshoon & Zhentarim|Waterdeep, Manshoon & Zhentarim]]
- [[_COMMUNITY_Aboleths, Ashmadai & Helm's Hold|Aboleths, Ashmadai & Helm's Hold]]
- [[_COMMUNITY_Pantheon, Alustriel & Spellplague|Pantheon, Alustriel & Spellplague]]
- [[_COMMUNITY_Ancient Fey & Neverwinter Wood|Ancient Fey & Neverwinter Wood]]
- [[_COMMUNITY_Silverymoon & Three-Faction Rule|Silverymoon & Three-Faction Rule]]
- [[_COMMUNITY_Daggerford & the Succubus Plot|Daggerford & the Succubus Plot]]
- [[_COMMUNITY_Dagult & the Alagondar Claim|Dagult & the Alagondar Claim]]
- [[_COMMUNITY_Netheril, Shar & Gray Wolves|Netheril, Shar & Gray Wolves]]
- [[_COMMUNITY_Baldur's Gate & Flaming Fist|Baldur's Gate & Flaming Fist]]
- [[_COMMUNITY_Magic Item Pricing|Magic Item Pricing]]
- [[_COMMUNITY_Thay & the Dead Rats|Thay & the Dead Rats]]
- [[_COMMUNITY_Yartar & Kraken Society|Yartar & Kraken Society]]
- [[_COMMUNITY_Neverneath & Castle Never Depths|Neverneath & Castle Never Depths]]
- [[_COMMUNITY_Arcane Acquisitions Shop|Arcane Acquisitions Shop]]
- [[_COMMUNITY_Beached Leviathan Crew|Beached Leviathan Crew]]
- [[_COMMUNITY_Longsaddle & the Harpells|Longsaddle & the Harpells]]
- [[_COMMUNITY_Luskan's Five High Captains|Luskan's Five High Captains]]
- [[_COMMUNITY_Cloak Tower & Many-Arrows|Cloak Tower & Many-Arrows]]
- [[_COMMUNITY_Campaign Guide Index|Campaign Guide Index]]
- [[_COMMUNITY_Fires Below Cult|Fires Below Cult]]
- [[_COMMUNITY_Seldra's Fake-Crown Plot (1479)|Seldra's Fake-Crown Plot (1479)]]
- [[_COMMUNITY_Castle Never Atrium|Castle Never Atrium]]
- [[_COMMUNITY_The Alagondar Crown|The Alagondar Crown]]
- [[_COMMUNITY_Seal of Neverwinter|Seal of Neverwinter]]
- [[_COMMUNITY_Chapter 3 Factions and Foes|Chapter 3: Factions and Foes]]
- [[_COMMUNITY_Chapter 4 Gazetteer|Chapter 4: Gazetteer]]
- [[_COMMUNITY_Old Owl Well|Old Owl Well]]
- [[_COMMUNITY_Shade Enclave|Shade Enclave]]
- [[_COMMUNITY_Surcross|Surcross]]
- [[_COMMUNITY_Shadowfell Road|Shadowfell Road]]
- [[_COMMUNITY_Dreamthief Doll|Dreamthief Doll]]
- [[_COMMUNITY_Umberlee (Sea Goddess)|Umberlee (Sea Goddess)]]
- [[_COMMUNITY_Lolth (Drow Goddess)|Lolth (Drow Goddess)]]
- [[_COMMUNITY_Laduguer (Duergar God)|Laduguer (Duergar God)]]
- [[_COMMUNITY_Nasher Takes Throne (1329 DR)|Nasher Takes Throne (1329 DR)]]
- [[_COMMUNITY_The Wailing Death (1372 DR)|The Wailing Death (1372 DR)]]
- [[_COMMUNITY_Dagult Neverember Born (1429 DR)|Dagult Neverember Born (1429 DR)]]
- [[_COMMUNITY_The Chasm Sealed (1486 DR)|The Chasm Sealed (1486 DR)]]
- [[_COMMUNITY_Dagult Neverember Disappears (1502 DR)|Dagult Neverember Disappears (1502 DR)]]
- [[_COMMUNITY_Renaer Ascends (1520 DR)|Renaer Ascends (1520 DR)]]
- [[_COMMUNITY_Castle Never Hall of Mirrors|Castle Never Hall of Mirrors]]
- [[_COMMUNITY_Vault of the Nine|Vault of the Nine]]
- [[_COMMUNITY_Lylorn (City)|Lylorn (City)]]
- [[_COMMUNITY_Fisher's Float|Fisher's Float]]
- [[_COMMUNITY_Grand Scribe Spivey Liethennson|Grand Scribe Spivey Liethennson]]
- [[_COMMUNITY_Loremaster Atlavast|Loremaster Atlavast]]
- [[_COMMUNITY_Alethea Brandarth|Alethea Brandarth]]

## God Nodes (most connected - your core abstractions)
1. `Neverwinter` - 77 edges
2. `Waterdeep` - 56 edges
3. `Lords' Alliance` - 44 edges
4. `Silverymoon` - 37 edges
5. `Dagult Neverember` - 32 edges
6. `Claugiyliamatar (Old Gnawbone)` - 32 edges
7. `Neverwinter Campaign (Campaign Home)` - 30 edges
8. `Mithral Hall` - 30 edges
9. `Manshoon` - 30 edges
10. `Renaer Neverember` - 29 edges

## Surprising Connections (you probably didn't know these)
- `Location Template` --semantically_similar_to--> `Neverwinter`  [INFERRED] [semantically similar]
  _Templates/Location Template.md → Campaign Home.md
- `Neverwinter City Walls` --part_of--> `Neverwinter`  [INFERRED]
  Maps/NeverwinterMapLabels-scaled.png → Campaign Home.md
- `Fisher's Feast` --located_in--> `Docks District`  [INFERRED]
  Maps/NeverwinterMapLabels-scaled.png → Campaign Home.md
- `Beached Leviathan` --located_in--> `Docks District`  [INFERRED]
  Maps/NeverwinterMapLabels-scaled.png → Campaign Home.md
- `Fallen Tower` --located_in--> `Tower/River District`  [INFERRED]
  Maps/NeverwinterMapLabels-scaled.png → Campaign Home.md

## Hyperedges (group relationships)
- **Lords' Alliance Member Cities (c. 1489 DR)** — waterdeep, neverwinter, silverymoon, baldurs_gate, mirabar, mithral_hall, yartar, daggerford, longsaddle, amphail, lords_alliance [EXTRACTED 1.00]
- **Campaign Magic Item Pricing System** — sane_magical_prices, rhexx_magic_items, sane_magic_market, house_magic_item_prices, magic_item_price_comparison, rhexx_enchantments_reference [EXTRACTED 1.00]
- **Waterdeep: Dragon Heist Outcome (1492 DR, winter branch)** — dragon_heist_party, renaer_neverember, manshoons_simulacrum, manshoon, dagults_hidden_hoard, vault_of_dragons, laeral_silverhand, waterdeep_dragon_heist [EXTRACTED 1.00]
- **Three-Faction Mapping of the Shadowy Force Investigation** — three_faction_rule, city_guard, arcane_brotherhood, the_harpers, shadowy_force [EXTRACTED 1.00]
- **Dragon Heist 1492 DR Outcome (Simulacrum Beaten, Hoard Returned)** — dragon_heist, manshoon, manshoons_simulacrum, renaer_neverember, dagults_hoard, vault_of_dragons, waterdeep [EXTRACTED 1.00]
- **Neverwinter Succession Crisis (1502-1520 DR)** — dagult_disappearance, dance_of_the_revolving_lords, renaer_ascension, the_powder, city_guard [EXTRACTED 1.00]

## Communities

### Community 0 - "Neverwinter Core & Renaer"
Cohesion: 0.04
Nodes (128): The Arcane Brotherhood, Beached Leviathan, Blacklake District, Bluelake (formerly Blacklake), Bluelake District, Calendar of Harptos, Campaign Decisions, Neverwinter Campaign (Campaign Home) (+120 more)

### Community 1 - "Dragons & Cult of the Dragon"
Cohesion: 0.03
Nodes (95): Air Spores (Woolsour's tool), Arauthator (Old White Death), Arveiaturace - The White Wyrm, Augathra the Mad, Balagos (red dragon rival), The House Brokengulf Con (Claugiyliamatar's playbook), Candlekeep, The Candlekeep Incident (+87 more)

### Community 2 - "Lords' Alliance & Northern Cities"
Cohesion: 0.03
Nodes (93): Agrathan Hardhammer, AideDD Faerûn Interactive Map, Amphail, Amphail the Just, Auril, Axe of Mirabar, Banak Brawnanvil, Barri Baram (+85 more)

### Community 3 - "Waterdeep, Manshoon & Zhentarim"
Cohesion: 0.04
Nodes (78): Agorn Fuoco, Ahghairon the Mage, Amath Sercent, Arveiaturace's Portal Battle, Bane, Blackstaff Tower, Brandath Mausoleum, City Watch of Waterdeep (+70 more)

### Community 4 - "Aboleths, Ashmadai & Helm's Hold"
Cohesion: 0.04
Nodes (58): Abeir (Twin World), Abolethic Sovereignty, Addemios Three-Dawn (Traitor), Alisara Callum (Chief Speaker), Ashmadai (Cult of Asmodeus), Ashmadai Cultists, Asmodeus, Bregan D'aerthe (+50 more)

### Community 5 - "Pantheon, Alustriel & Spellplague"
Cohesion: 0.07
Nodes (39): Alustriel Silverhand, Atlavast (Library Caretaker), King Bann Alagondar, Bhaal - Lord of Murder, Brother Anthus (Late Priest of Oghma), Brother Vartan (Priest of Oghma), Chauntea, Corellon - Protector of the Elves (+31 more)

### Community 6 - "Ancient Fey & Neverwinter Wood"
Cohesion: 0.05
Nodes (38): Ancient History of Neverwinter, Crown Wars, Dire Wood, Dread Ring (Thay), Eigersstor (Original Name), Eigersstor Founded (87 DR), Evermeet, Favria (Ashmadai Leader) (+30 more)

### Community 7 - "Silverymoon & Three-Faction Rule"
Cohesion: 0.08
Nodes (37): Alustriel, Argent Legion, Bad Faction, Church of Cyric Outlawed (Silverymoon), Citadel Adbar, Council of the Marches, Dissolution of Luruar (1488 DR), Duchy of Riverfell (+29 more)

### Community 8 - "Daggerford & the Succubus Plot"
Cohesion: 0.08
Nodes (33): Ambergul Blade, Baazka, Christos Tellum, Clan Ironeater, Council of Guilds (Daggerford), Cromm's Hold, Daggerford, Compulsory Militia Service (Daggerford) (+25 more)

### Community 9 - "Dagult & the Alagondar Claim"
Cohesion: 0.07
Nodes (32): The Alagondar Dynasty, Arlon Bladeshaper (Nashers Leader), Brandarth Hall (Waterdeep), Cymril, Disappearance of Dagult Neverember (1502 DR), Dagult Neverember, Dalakhar, Dunfield (Town Watch Captain) (+24 more)

### Community 10 - "Netheril, Shar & Gray Wolves"
Cohesion: 0.08
Nodes (29): Agatha (Elven Banshee), Anauroch, Beorunna's Well, Bey of Runlatha, Black Lion Tribe (Uthgardt), Clariburnus Tanthul (Prince of Shade), Conyberry, Group Opposing the Gods' Overreach (+21 more)

### Community 11 - "Baldur's Gate & Flaming Fist"
Cohesion: 0.1
Nodes (28): Abdel Adrian, Amn, Avernus, Baldur's Gate, Baldur's Gate 3, Baldur's Gate Code of Law, The Watch (Baldur's Gate), Council of Four (+20 more)

### Community 12 - "Magic Item Pricing"
Cohesion: 0.17
Nodes (26): Abundant Magic (campaign decision), Amulet of Proof against Detection and Location, Arcane Acquisitions, Bag of Holding, Bind Spell (Rhexx enchantment), Boots of the Winterlands, Critical Role Campaign 4 (Araman), Defensive Enhancement (Rhexx enchantment) (+18 more)

### Community 13 - "Thay & the Dead Rats"
Cohesion: 0.09
Nodes (26): Adimond Kroskas (Cult of the Dragon), Aganazzar, Arklem Greeth, Asmodeus Scepter (Ashmadai Relic), Dead Rats Gang, Dread Ring, Drizzt Do'Urden, Grimwald (+18 more)

### Community 14 - "Yartar & Kraken Society"
Cohesion: 0.12
Nodes (24): Alahar Khaumfros, Belleethe Kheldorna, Caladorn Cassalanter, Devastation Orb, Elemental Evil Crisis (1491 DR), Evermoor Way, Hand of Yartar, Happy Hall of Fortuitous Happenstance (+16 more)

### Community 15 - "Neverneath & Castle Never Depths"
Cohesion: 0.15
Nodes (17): Alagondar Bloodline Lock, Crypt of the Neverwinter Nine, Halueth Never Settles Region (-10 DR), Halueth Never, Halueth Never's Stone Slab, Illusk (Ancient Ruin), Inner Vault (Castle Never), Maerin (Mage of Illusk) (+9 more)

### Community 16 - "Arcane Acquisitions Shop"
Cohesion: 0.12
Nodes (17): Airy Leather Armor, Arcane Acquisitions (Shop), Babel Earring, Bartender's Boots, Deerstalker Hat, Defensive Cleaver, Enraged Dagger, Extra-Light Longbow (+9 more)

### Community 17 - "Beached Leviathan Crew"
Cohesion: 0.13
Nodes (16): Andrella (Mate), Bobrik Highmantle (Bosun), Captain Harrag, Ettain (Caulker), Feng (Bouncer), Harrag's Animated Sentient Shadow, Korin (Chanteyman), Len-jes (Harbormaster) (+8 more)

### Community 18 - "Longsaddle & the Harpells"
Cohesion: 0.21
Nodes (15): Ardanac Harpell, Bidderdoo Harpell, The Bidderdoos, Blue Sigil Mercenary Company, Catti-brie, Dowell Harpell, Harpell Family, Ivy Mansion (+7 more)

### Community 19 - "Luskan's Five High Captains"
Cohesion: 0.29
Nodes (7): Emperor of the Seas, The Five High Captains, Ship Baram, Ship Kurth, Ship Rethnor, Ship Suljack, Ship Taerl

### Community 20 - "Cloak Tower & Many-Arrows"
Cohesion: 0.29
Nodes (7): Cloak Tower Vanishing (1385 DR), Halavar's Universal Pantograph, Llomnauvel 'Firehands' Oloadhin, Many-Arrows Tribe, The Cloak Tower, The Fallen Tower (Tavern), Vagdru One-Ear

### Community 21 - "Campaign Guide Index"
Cohesion: 0.67
Nodes (3): Neverwinter Campaign Setting Guide, Chapter 1: Jewel of the North, Chapter 2: Character Options

### Community 22 - "Fires Below Cult"
Cohesion: 0.67
Nodes (3): Gommoth (Fire Giant), Qalthus (Shadovar Mage), The Fires Below (Cult)

### Community 23 - "Seldra's Fake-Crown Plot (1479)"
Cohesion: 0.67
Nodes (3): Replica Crown of Neverwinter, Seldra's False-Crown Plot (1479 DR), Seldra Tylmarande

### Community 24 - "Castle Never Atrium"
Cohesion: 1.0
Nodes (2): Atrium Myconids, Castle Never Atrium

### Community 25 - "The Alagondar Crown"
Cohesion: 1.0
Nodes (1): The Alagondar Crown

### Community 26 - "Seal of Neverwinter"
Cohesion: 1.0
Nodes (1): Seal of Neverwinter

### Community 27 - "Chapter 3: Factions and Foes"
Cohesion: 1.0
Nodes (1): Chapter 3: Factions and Foes

### Community 28 - "Chapter 4: Gazetteer"
Cohesion: 1.0
Nodes (1): Chapter 4: Gazetteer

### Community 29 - "Old Owl Well"
Cohesion: 1.0
Nodes (1): Old Owl Well

### Community 30 - "Shade Enclave"
Cohesion: 1.0
Nodes (1): Shade Enclave

### Community 31 - "Surcross"
Cohesion: 1.0
Nodes (1): Surcross

### Community 32 - "Shadowfell Road"
Cohesion: 1.0
Nodes (1): Shadowfell Road

### Community 33 - "Dreamthief Doll"
Cohesion: 1.0
Nodes (1): Dreamthief Doll

### Community 34 - "Umberlee (Sea Goddess)"
Cohesion: 1.0
Nodes (1): Umberlee (Sea Goddess)

### Community 35 - "Lolth (Drow Goddess)"
Cohesion: 1.0
Nodes (1): Lolth (Drow Goddess)

### Community 36 - "Laduguer (Duergar God)"
Cohesion: 1.0
Nodes (1): Laduguer (Duergar God)

### Community 37 - "Nasher Takes Throne (1329 DR)"
Cohesion: 1.0
Nodes (1): Nasher Takes Throne (1329 DR)

### Community 38 - "The Wailing Death (1372 DR)"
Cohesion: 1.0
Nodes (1): The Wailing Death (1372 DR)

### Community 39 - "Dagult Neverember Born (1429 DR)"
Cohesion: 1.0
Nodes (1): Dagult Neverember Born (1429 DR)

### Community 40 - "The Chasm Sealed (1486 DR)"
Cohesion: 1.0
Nodes (1): The Chasm Sealed (1486 DR)

### Community 41 - "Dagult Neverember Disappears (1502 DR)"
Cohesion: 1.0
Nodes (1): Dagult Neverember Disappears (1502 DR)

### Community 42 - "Renaer Ascends (1520 DR)"
Cohesion: 1.0
Nodes (1): Renaer Ascends (1520 DR)

### Community 43 - "Castle Never Hall of Mirrors"
Cohesion: 1.0
Nodes (1): Castle Never Hall of Mirrors

### Community 44 - "Vault of the Nine"
Cohesion: 1.0
Nodes (1): Vault of the Nine

### Community 45 - "Lylorn (City)"
Cohesion: 1.0
Nodes (1): Lylorn (City)

### Community 46 - "Fisher's Float"
Cohesion: 1.0
Nodes (1): Fisher's Float

### Community 47 - "Grand Scribe Spivey Liethennson"
Cohesion: 1.0
Nodes (1): Grand Scribe Spivey Liethennson

### Community 48 - "Loremaster Atlavast"
Cohesion: 1.0
Nodes (1): Loremaster Atlavast

### Community 49 - "Alethea Brandarth"
Cohesion: 1.0
Nodes (1): Alethea Brandarth

## Ambiguous Edges - Review These
- `Neverwinter` → `Zhentarim`  [AMBIGUOUS]
  NPCs/Manshoon.md · relation: conceptually_related_to
- `Luskan` → `The Shadowy Force`  [AMBIGUOUS]
  Locations/Luskan.md · relation: investigates
- `Dagult Neverember` → `Zundaerazylym (The Laughing Wyrm)`  [AMBIGUOUS]
  NPCs/Zundaerazylym - The Laughing Wyrm.md · relation: conceptually_related_to
- `Madam Ophala Cheldarstorn` → `Mystra`  [AMBIGUOUS]
  NPCs/Ophala Cheldarstorn.md · relation: ally_of
- `Claugiyliamatar (Old Gnawbone)` → `Shar`  [AMBIGUOUS]
  NPCs/Selune and Shar - Sister Goddesses.md · relation: ally_of
- `Emperor of the Seas` → `The Arcane Brotherhood`  [AMBIGUOUS]
  Factions/Arcane Brotherhood.md · relation: controls
- `The Harpers` → `Zundaerazylym (The Laughing Wyrm)`  [AMBIGUOUS]
  NPCs/Zundaerazylym - The Laughing Wyrm.md · relation: enemy_of
- `The Arcane Brotherhood` → `The Shadowy Force`  [AMBIGUOUS]
  Lore/3 Faction Rule.md · relation: conceptually_related_to
- `The Arcane Brotherhood` → `Kurtulmak's Chosen`  [AMBIGUOUS]
  Lore/3 Faction Rule.md · relation: allied_with
- `Cult of the Dragon` → `Cult of Myrkul`  [AMBIGUOUS]
  Lore/Adventure - Sleeping Dragon's Wake.md · relation: conceptually_related_to
- `Sharran Cult` → `Permanent Human-Form Ritual (Claugiyliamatar's goal)`  [AMBIGUOUS]
  NPCs/Selune and Shar - Sister Goddesses.md · relation: ally_of
- `Mere of Dead Men` → `Iniarv`  [AMBIGUOUS]
  Lore/Sword Coast Reference.md · relation: located_in
- `Cataclysm of 1451 DR (Mount Hotenow Eruption)` → `Abolethic Sovereignty`  [AMBIGUOUS]
  Lore/The Three Cataclysms.md · relation: caused
- `Sword Mountains` → `Kurtulmak's Chosen`  [AMBIGUOUS]
  Lore/Sword Coast Reference.md · relation: located_in
- `Mordai Vell (Ashmadai Leader)` → `Rohini the Prophet`  [AMBIGUOUS]
  Lore/Campaign_Guide/Chapter_4_Gazetteer.md · relation: ally_of
- `Vers Never (Alleged Bastard Son)` → `House Neverember`  [AMBIGUOUS]
  NPCs/Dagult Neverember.md · relation: allegedly_founded_by
- `Amphail` → `House Ilzimmer`  [AMBIGUOUS]
  Locations/Amphail.md · relation: rules
- `Amphail` → `House Amcathra`  [AMBIGUOUS]
  Locations/Amphail.md · relation: rules
- `Amphail` → `House Roaringhorn`  [AMBIGUOUS]
  Locations/Amphail.md · relation: rules
- `The Cloak Tower` → `Halavar's Universal Pantograph`  [AMBIGUOUS]
  Lore/Neverwinter Sub-Locations.md · relation: located_in
- `Neverwinter Mining House` → `The Powder`  [AMBIGUOUS]
  Campaign Decisions.md · relation: conceptually_related_to
- `The Powder` → `Manshoon`  [AMBIGUOUS]
  NPCs/Manshoon.md · relation: conceptually_related_to
- `The 1492 Dragon Heist Party` → `Manshoon's Ledger`  [AMBIGUOUS]
  Campaign Decisions.md · relation: conceptually_related_to
- `The 1492 Dragon Heist Party` → `Jelenn Urmbrusk`  [AMBIGUOUS]
  Campaign Decisions.md · relation: allied_with
- `Hand of Yartar` → `Kaidrod Palyr`  [AMBIGUOUS]
  Locations/Yartar.md · relation: conceptually_related_to
- `Selin Ramur` → `Elastul Raurym`  [AMBIGUOUS]
  Locations/Mirabar.md · relation: conceptually_related_to
- `Neverneath` → `Zundaerazylym (The Laughing Wyrm)`  [AMBIGUOUS]
  NPCs/Zundaerazylym - The Laughing Wyrm.md · relation: conceptually_related_to

## Knowledge Gaps
- **336 isolated node(s):** `Balagos (red dragon rival)`, `Endracritar (rival green dragon)`, `Smergadas (young red dragon)`, `Hyrix Greentree (wizard)`, `King Uth VII of Uthtower` (+331 more)
  These have ≤1 connection - possible missing edges or undocumented components.
- **Thin community `Castle Never Atrium`** (2 nodes): `Atrium Myconids`, `Castle Never Atrium`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `The Alagondar Crown`** (1 nodes): `The Alagondar Crown`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Seal of Neverwinter`** (1 nodes): `Seal of Neverwinter`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Chapter 3: Factions and Foes`** (1 nodes): `Chapter 3: Factions and Foes`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Chapter 4: Gazetteer`** (1 nodes): `Chapter 4: Gazetteer`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Old Owl Well`** (1 nodes): `Old Owl Well`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Shade Enclave`** (1 nodes): `Shade Enclave`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Surcross`** (1 nodes): `Surcross`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Shadowfell Road`** (1 nodes): `Shadowfell Road`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Dreamthief Doll`** (1 nodes): `Dreamthief Doll`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Umberlee (Sea Goddess)`** (1 nodes): `Umberlee (Sea Goddess)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Lolth (Drow Goddess)`** (1 nodes): `Lolth (Drow Goddess)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Laduguer (Duergar God)`** (1 nodes): `Laduguer (Duergar God)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Nasher Takes Throne (1329 DR)`** (1 nodes): `Nasher Takes Throne (1329 DR)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `The Wailing Death (1372 DR)`** (1 nodes): `The Wailing Death (1372 DR)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Dagult Neverember Born (1429 DR)`** (1 nodes): `Dagult Neverember Born (1429 DR)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `The Chasm Sealed (1486 DR)`** (1 nodes): `The Chasm Sealed (1486 DR)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Dagult Neverember Disappears (1502 DR)`** (1 nodes): `Dagult Neverember Disappears (1502 DR)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Renaer Ascends (1520 DR)`** (1 nodes): `Renaer Ascends (1520 DR)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Castle Never Hall of Mirrors`** (1 nodes): `Castle Never Hall of Mirrors`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Vault of the Nine`** (1 nodes): `Vault of the Nine`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Lylorn (City)`** (1 nodes): `Lylorn (City)`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Fisher's Float`** (1 nodes): `Fisher's Float`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Grand Scribe Spivey Liethennson`** (1 nodes): `Grand Scribe Spivey Liethennson`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Loremaster Atlavast`** (1 nodes): `Loremaster Atlavast`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.
- **Thin community `Alethea Brandarth`** (1 nodes): `Alethea Brandarth`
  Too small to be a meaningful cluster - may be noise or needs more connections extracted.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **What is the exact relationship between `Neverwinter` and `Zhentarim`?**
  _Edge tagged AMBIGUOUS (relation: conceptually_related_to) - confidence is low._
- **What is the exact relationship between `Luskan` and `The Shadowy Force`?**
  _Edge tagged AMBIGUOUS (relation: investigates) - confidence is low._
- **What is the exact relationship between `Dagult Neverember` and `Zundaerazylym (The Laughing Wyrm)`?**
  _Edge tagged AMBIGUOUS (relation: conceptually_related_to) - confidence is low._
- **What is the exact relationship between `Madam Ophala Cheldarstorn` and `Mystra`?**
  _Edge tagged AMBIGUOUS (relation: ally_of) - confidence is low._
- **What is the exact relationship between `Claugiyliamatar (Old Gnawbone)` and `Shar`?**
  _Edge tagged AMBIGUOUS (relation: ally_of) - confidence is low._
- **What is the exact relationship between `Emperor of the Seas` and `The Arcane Brotherhood`?**
  _Edge tagged AMBIGUOUS (relation: controls) - confidence is low._
- **What is the exact relationship between `The Harpers` and `Zundaerazylym (The Laughing Wyrm)`?**
  _Edge tagged AMBIGUOUS (relation: enemy_of) - confidence is low._