# DeadAir Economy Overhaul — mlog4 fork — Changelog

> **English version below. Русская версия ниже.**

---

## English

This is a community-maintained compatibility fork of **DeadAir Economy Overhaul** by DeadAirRT, distributed under the GPL-3.0 licence the original author left when retiring (2025-09-23). The original gameplay logic is preserved unchanged — this fork only fixes the (substantial) XPath drift that had crept in between the mod's last release and current X4 vanilla.

**Companion mod** (required): [DeadAir Scripts — mlog4 fork](https://github.com/mlog4/deadair_scripts). The two mods are tested as a pair.

### v1.13.1-beta1 — 2026-04-29

**Tested on**: X4 Foundations **9.0 beta 7**.
**Save-game**: should be compatible with existing saves (all changes are XML patches; no save-state schema changes). After installing this build, NPC stations being newly *constructed* will use the (now correctly applied) larger DA layout — see "Important behavioural change" below.
**Upstream baseline**: DeadAir Economy Overhaul v1.13HF1 (DeadAirRT, retired 2025-09-23).

#### Important behavioural change — DA's intended NPC station design now actually applies

Seven of DA-Eco's largest patches were silently failing throughout the X4 8.x → 9.x era (an XPath in their selector pointed into a structure that vanilla had since wrapped in a new branch). They have been re-aligned and now apply for the first time. Effect:

- **Bigger NPC factories.** Per-station production-module limit for non-recycling wares: 1 → 5 (DA's "build bigger factories" intent restored).
- **More clearance around new stations.** Safe-position search radius widened (from 3 km to 21 km), reducing module-clipping and collision spawns.
- **Multi-location factory expansions.** DA's `$DALocCount min=1 max=2` randomly adds 1–2 extra production macros per expansion — bigger station growth.
- **Larger built-in storage.** `DAIncreaseStartingStorage` library — adding ~12 h of production storage and ~2 h of resource storage to new stations — is now actually invoked.
- **Race-specific structural connectors.** `DAImproveStationLayouts` library now applies for plans of ≥10 modules.

This is a substantial deviation from how the mod has *behaved* on X4 8.x for the last year. It matches what DA originally intended, but if your existing save was balanced around the previously-broken behaviour, expect bigger NPC stations going forward. **Watch for**: very large stations, station-spam, save-bloat from longer construction sequences. If any of those happen, please report on the GitHub issue tracker.

#### Other player-visible fixes

- **Mining efficiency 90 % now actually applies.** DA's intended slowdown of NPC mining (from 100 % to 90 %) was being silently no-op'd — its XPath traversed a `<do_while>` that vanilla 9.x moved into a new `<do_else>` branch. Now correctly applies. NPC mining ships are slightly slower than vanilla.
- **NPC galaxy quotas restored.** 16 god-script quotas for production-module placement (food rations, graphene, hull parts, refined metals, claytronics, silicon wafers, metallic microlattice across multiple races) now match the current vanilla quota fields they are meant to override. Without the fix, DA's quota intent was silently failing — galaxy-wide production-module distribution would default to vanilla.
- **Argon advanced-composites / claytronics economy thresholds restored.** DA replaces an `<economy max="0.75">` filter with a `<security min="0.5" max="0.75">` filter on these wares. The selector now matches vanilla's tightened threshold (0.50 instead of 0.75) so the replacement actually fires.

#### Cleanup — patches removed where vanilla now does the same thing

These patches were dead code: vanilla X4 9.x had already converged on what DA-Eco was trying to enforce, so the patches just produced load-time errors without changing anything. **No gameplay change** vs. how the mod actually behaved before this beta.

- **69 jobs patches removed** (23 medium-class trader jobs × 3 patches each — `Middleman` → `TradeRoutine` rename + param renames + `maxsell` add). Vanilla 9.x ships with this exact structure natively.
- **62 region-yield patches removed.** Vanilla X4 8.x changed the schema for asteroid yield definitions (now expressed via `<asteroid densityfactor=…/>` inside `<fields>`, not via the old `<resources>/<resource yield=…/>` block). DA-Eco's 7.x-era yield rebalance can no longer be expressed against vanilla — it is **suspended** with this build. Practical effect: zero, since these patches had been failing since X4 8.x. Reimplementing yield rebalance against the new schema is a possible future project.
- **6 dead constructionplans / baskets patches removed.** Paranid trading-station "lowtech-pier upgrade" (vanilla restructured the plan; the lowtech docks are no longer there) and refined/surface basket energycells removes (vanilla already excludes them).
- **1 dead "boron advancedcomposites cross-faction" patch removed.** Vanilla removed the `@faction` filter the patch was trying to rewrite.

#### Cleanup (debug.log)

- Total `=ERROR=` patch failures from DA-Eco against vanilla 9.x: **~200 / session → 0**.

#### Known issues / not yet addressed

- X4 9.0 release version not yet tested (currently testing on 9.0 beta 7). When 9.0 ships we will retest.
- Yield-rebalance feature is suspended (see "62 region-yield patches removed" above). If you specifically want DA-Eco's hand-tuned per-region resource yields, you currently get vanilla yields instead. Reimplementation against the new schema is a future project.
- Galaxy-quota patches preserve DA's intent against vanilla 9.x quota *targets*, but as vanilla continues to evolve quotas, more drift may surface in future X4 versions. Will revisit when 9.x sees content updates.

#### Credits

- Original mod: **DeadAirRT**.
- v1.17 community fork: **Sleaker** (`v1.17_sleaker_no_da_wares` is in our reference tree; this fork descends from DA v1.13HF1 not v1.17).
- 9.x compatibility maintenance: **mlog4**.

#### Licence

GPL-3.0 inherited from upstream. The original `LICENSE` file is preserved unchanged in this repository.

---

## Русский

Это форк-поддержка совместимости мода **DeadAir Economy Overhaul** от DeadAirRT, распространяется по лицензии GPL-3.0 которую автор оставил уходя (23.09.2025). Геймплейная логика оригинала сохранена без изменений — этот форк чинит (значительный) XPath drift, накопившийся между последним релизом мода и текущей vanilla.

**Парный мод** (обязательно): [DeadAir Scripts — mlog4 fork](https://github.com/mlog4/deadair_scripts). Оба мода тестировались как пара.

### v1.13.1-beta1 — 29.04.2026

**Тестировалось на**: X4 Foundations **9.0 beta 7**.
**Сейв-игры**: должны быть совместимы (все изменения — XML-патчи, схема сохранения не трогается). После установки этого билда новостроящиеся NPC-станции будут использовать (теперь корректно применяющийся) больший DA-лейаут — см. «Важное изменение поведения» ниже.
**Upstream-базовая версия**: DeadAir Economy Overhaul v1.13HF1 (DeadAirRT, retired 23.09.2025).

#### Важное изменение поведения — задуманный DA-ом дизайн NPC-станций теперь реально применяется

Семь самых крупных патчей DA-Eco тихо проваливались всё время существования X4 8.x → 9.x (XPath в их селекторе указывал на структуру которую vanilla давно завернул в новую ветку). Они выровнены и теперь применяются впервые. Эффект:

- **Бо́льшие NPC-фабрики.** Лимит производственных модулей одного типа на станцию (для не-recycling товаров): 1 → 5 (восстановлено намерение DA «строить большие фабрики»).
- **Больше места вокруг новых станций.** Радиус поиска безопасной позиции расширен (с 3 км до 21 км), уменьшая клиппинг модулей и коллизии при спавне.
- **Multi-location расширения фабрик.** DA-евский `$DALocCount min=1 max=2` рандомно добавляет 1–2 дополнительных производственных макроса при расширении — больший рост станций.
- **Больше встроенного хранилища.** Библиотека `DAIncreaseStartingStorage` (добавляет ~12 ч production-storage и ~2 ч resource-storage новым станциям) теперь реально вызывается.
- **Расовые конструктивные коннекторы.** Библиотека `DAImproveStationLayouts` теперь применяется для планов ≥10 модулей.

Это существенное отклонение от того как мод *вёл* себя на X4 8.x последний год. Совпадает с тем что DA изначально планировал, но если ваш сейв был сбалансирован под ранее-сломанное поведение — ожидайте увеличение NPC-станций. **Следите за**: очень большие станции, станц-спам, разрастание сейва из-за более длинных construction-последовательностей. Если что-то из этого происходит — отпишитесь в GitHub Issues.

#### Прочие исправления видимые игроку

- **Mining efficiency 90 % теперь реально применяется.** DA-евский задуманный slowdown NPC-добычи (со 100 % до 90 %) тихо проваливался — его XPath проходил через `<do_while>` который vanilla 9.x перенёс в новую ветку `<do_else>`. Теперь корректно применяется. NPC mining-ships слегка медленнее vanilla.
- **NPC galaxy-quotas восстановлены.** 16 god-скриптовых quotas для размещения production-модулей (food rations, graphene, hull parts, refined metals, claytronics, silicon wafers, metallic microlattice по разным расам) теперь соответствуют текущим vanilla-quota полям которые они должны переопределять. Без фикса quota-задумка DA тихо падала — распределение производственных модулей по галактике скатывалось в vanilla.
- **Argon advanced-composites / claytronics economy thresholds восстановлены.** DA заменяет фильтр `<economy max="0.75">` на `<security min="0.5" max="0.75">` для этих товаров. Селектор теперь матчится с обновлённым vanilla-порогом (0.50 вместо 0.75) — замена реально срабатывает.

#### Чистка — удалены патчи, потому что vanilla теперь делает то же самое

Эти патчи были мёртвым кодом: vanilla X4 9.x уже сошёлся на том что DA-Eco пыталась сделать, так что патчи только генерировали ошибки при загрузке без эффекта. **Изменения геймплея нет** относительно того как мод реально вёл себя до этой беты.

- **Удалено 69 jobs-патчей** (23 средне-класс trader-джобов × 3 патча — переименование `Middleman` → `TradeRoutine`, переименование параметров, добавление `maxsell`). Vanilla 9.x уже имеет ровно такую структуру нативно.
- **Удалено 62 region-yield патчей.** Vanilla X4 8.x изменил схему определения asteroid yields (теперь через `<asteroid densityfactor=…/>` внутри `<fields>`, а не через старый блок `<resources>/<resource yield=…/>`). DA-евский 7.x-перебаланс yield-ов больше нельзя выразить относительно vanilla — он **приостановлен** в этом билде. Практический эффект: нулевой, эти патчи проваливались с самого X4 8.x. Переписывание перебаланса yield под новую схему — возможный будущий проект.
- **Удалено 6 dead constructionplans / baskets патчей.** Paranid trading-station "lowtech-pier upgrade" (vanilla перестроил план, lowtech-доков там больше нет) и refined/surface basket energycells removes (vanilla уже их исключает).
- **Удалён 1 dead patch** для «boron advancedcomposites cross-faction». Vanilla убрал атрибут `@faction` который патч пытался переписать.

#### Чистка (debug.log)

- Суммарно ошибок `=ERROR=` от DA-Eco против vanilla 9.x: **~200 за сессию → 0**.

#### Известные проблемы / ещё не сделано

- Совместимость с релизной X4 9.0 пока не проверялась (тестируем на 9.0 beta 7). Когда выйдет релиз — перепроверим.
- Yield-rebalance feature приостановлен (см. «удалено 62 region-yield патчей» выше). Если вам конкретно нужны DA-евские руками-настроенные yields по регионам — сейчас получаете vanilla yields. Переписывание под новую схему — будущий проект.
- Galaxy-quota патчи сохраняют намерение DA относительно текущих vanilla 9.x quota-таргетов, но если vanilla продолжит эволюционировать — drift может всплыть в будущих версиях X4. Перепроверим при content-апдейтах 9.x.

#### Благодарности

- Оригинальный мод: **DeadAirRT**.
- Community-форк v1.17: **Sleaker** (`v1.17_sleaker_no_da_wares` есть в нашем дереве референсов; наш форк происходит от DA v1.13HF1, а не v1.17).
- Поддержка совместимости с 9.x: **mlog4**.

#### Лицензия

GPL-3.0, унаследована от upstream-а. Оригинальный файл `LICENSE` сохранён в репозитории без изменений.
