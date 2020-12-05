# TGR Models

Models for Tradizione Grammaticale Romana.

## Items

In this project there are just 2 types of items: text with layers, and manuscripts.

### Text

Text with layers: 4 parts/fragments are reused, 3 fragments are new.

- [token-based text part](https://github.com/vedph/cadmus_core/wiki/General-Parts#token-text)\* (`TokenTextPart`)
- [note part](https://github.com/vedph/cadmus_core/wiki/General-Parts#note) for generic notes (`NotePart`)
- [note part](https://github.com/vedph/cadmus_core/wiki/General-Parts#note) for translation, with `role`=`transl` and `tag`=language (`NotePart`).

- [apparatus layer](https://github.com/vedph/cadmus_core/wiki/Philology-Parts#apparatus-model) (`ApparatusLayerFragment`)
- [literary quotations layer](https://github.com/vedph/cadmus_core/wiki/Philology-Parts#quotations) (`QuotationsLayerFragment`) for _apparatus fontium et locorum classicorum_. Parallel quotations entries can be grouped under a common parent quotation using the `tag` property (e.g. tagging the parent as `prisc1` and its children as `prisc1.1`, `prisc1.2`, etc.). This avoids replicating the quotation schema with a potentially recursive nesting and simplifies the schema with a flat list.
- transcriptions layer (`TranscrLayerFragment`), used with different roles for:
  - paleographic transcriptions
  - glosses
  - paratext
- interpolations layer (`MsInterpLayerFragment`)
- linguistic tags layer (`LingTagsLayerFragment`)

### Manuscript

- [historical date](https://github.com/vedph/cadmus_core/wiki/General-Parts#historical-date-model)\*
- [generic bibliography part](https://github.com/vedph/cadmus_core/wiki/General-Parts#bibliography)\*
- ms signature(s)\* [Itinera ms signatures](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-signatures-part.md).
- ms place\*: [Itinera ms place of origin](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-place-part.md) (the places of provenance are listed in the ms history).
- ms contents\* (`MsContentPart`)
- ms codicological description\* (`MsUnitsPart`)
- ms history\* (`MsHistoryPart`)
- ms scripts part (`MsScriptsPart`)
- ms formal features (`MsFormalFeaturesPart`)
- ms ornamentation (`MsOrnamentsPart`)

## Parts

### MsSignaturesPart

See [Itinera](https://github.com/vedph/cadmus_itinera_doc/blob/master/models.md#mssignaturespart)

### MsPlacePart

See [Itinera](https://github.com/vedph/cadmus_itinera_doc/blob/master/models.md#msplacepart)

### MsContentsPart

- contents (`MsContent[]`):
  - start\* (`MsLocation`)
  - end\* (`MsLocation`)
  - citation\* (`string`): either got from ThlL (via author/work hierarchical thesaurus) or manually written.
  - title (`string`)
  - incipit\* (`string`)
  - editions (`DocReference[]`): references to modern editions.
  - note (`string`)

### MsUnitsPart

Codicological description.

- `units`: `MsUnit[]+` (codicological units):
  - `start`\*: `MsLocation`
  - `end`\*: `MsLocation`
  - `palimpsests` (`MsPalimpsest[]`): the sheet(s) which are palimpsests inside this unit:
    - `locations` (`MsLocation[]`): 1 or more sheets in the unit's range.
    - `date` (`HistoricalDate`)
    - `note` (`string`)
  - `material`\* (`string`, thesaurus)
  - `sheetCount`\* (`int`)
  - `guardSheetCount`\* (`int`)
  - `guardSheets` (`MsGuardSheet[]`):
    - `isBack` (`boolean`)
    - `material` (`string`)
    - `watermarks` (`MsWatermark[]`):
      - `value` (`string`)
      - `description` (`string`)
    - `note` (`string`)
  - `quires` (`string`)
  - `sheetNumbering` (`string`; leaf or page numbering)
  - `quireNumbering` (`string`)
  - `leafSizes` (`PhysicalSize[]`): note that the requested _incomplete_ property is rather represented by `tag`. Tag is a more general purpose classification and tagging device for each specific dimension: it may be "incomplete", and/or also additional values.
  - `writtenAreaSize` (`PhysicalSize`)
  - `rulings` (`MsRuling[]`)
    - `manner` of execution (`string`, thesaurus)
    - `system` (`string`, thesaurus)
    - `type` (`string`, thesaurus)
    - `description` (`string`)
  - `watermarks` (`MsWatermark[]`)
  - conservation `state` (`string`)
  - `binding` (`string`)

### MsScriptsPart

- `scripts` (`MsScript[]`):
  - `role`\* (`string`, thesaurus; e.g. mano principale, secondaria, scriptio superior, scriptio inferior...)
  - `language`\* (`string` [ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3), thesaurus)
  - `type` (`string`, thesaurus-h)
  - `hands` (`MsHand[]`+):
    - `id`\* (`string`)
    - `datation` (`HistoricalDate`)
    - `start`\* (`MsLocation`)
    - `end`\* (`MsLocation`)
    - `description`\* (`string`)
    - `letters` (`MsHandLetter[]`):
      - `letter`\* (`string`)
      - `description` (`string`)
      - `imageId` (`string`)
    - `abbreviations` (`string`, MD)

### MsFormalFeaturesPart

Orthographic/phonetic peculiarities:

- `features` (`MsFormalFeature[]`):
  - `description`\* (`string`, MD)
  - `handId`\* (`string`)

### MsOrnamentsPart

Ornamentation (in code shortened as _ornament_. for practical purposes).

- `ornaments` (`MsOrnament[]`):
  - `type`\* (`string`, thesaurus)
  - `start`\* (`MsLocation`)
  - `end`\* (`MsLocation`)
  - `size` (`PhysicalSize`)
  - `description` (`string`, MD, 1000)

### MsHistoryPart

- `provenances`\* ([GeoAddress[]](https://github.com/vedph/cadmus_itinera_doc/blob/master/models.md#mshistorypart))
- `history`\* (`string`, MD, 5000)
- `owners` (`string[]`)
- `subscription` (`MsSubscription`):
  - `locations`\* (`MsLocation[]`)
  - `language`\* (`string`, thesaurus)
  - `text`\* (`string`)
  - `note` (`string`)
  - `handId` (`string`)
- `annotations` and corrections (`MsAnnotation[]`):
  - `language`\* (`string`, thesaurus)
  - `note` (`string`)
  - `handId` (`string`)

### MsInterpLayerFragment

Humanistic interpolations.

- `entries`: `MsInterpolationEntry[]`:
  - `type` (thesaurus): `string`\*
  - `language`: `string` ([ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3))
  - `value`: `string`\*
  - `tag` (thesaurus): `string`
  - `groupId`: `string`
  - `sources`: `MsReadingSource[]`+
    - `witness` (thesaurus): `string`\*
    - `handId`: `string`
  - `quotations`: `QuotationEntry[]`

### TranscrLayerFragment

- entries: `TranscriptionEntry[]`:
  - type (thesaurus): string\*
  - language (thesaurus): string ([ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3))
  - value: string\*
  - tag (thesaurus): string
  - groupId: string
  - sources: `MsReadingSource[]`+
    - witness (thesaurus): string\*
    - handId: string

### LingTagsLayerFragment

Tag: `fr.it.vedph.tgr.ling-tags`.

- `forms`: `LingTaggedForm[]`:
  - `lemmata`: `string[]`: optional normalized text forms
  - `dubious`: `boolean` (Thesaurus dubii sermonis)
  - `note`: string
  - `tags`: `AnnotatedTag[]`:
    - `value`: `string`\*
    - `notes`: `TaggedNote[]`:
      - `tag`: `string`\*
      - `note`: `string`\*

The user experience for tags is here planned as follows: the user picks tags from a tree. The thesaurus IDs use the dot as the separator to represent a hierarchy, e.g. `a.lit.l-sonus.medius` = ancient / littera / l-sonus / medius.

Every tag picked from the tree is stored in a list. A button next to each picked tag in this list allows removing it.

Also, some tags can be annotated, and their annotations types are explicitly defined. This is done in another auxiliary thesaurus, whose ID is equal to the ID feeding this fragment editor, plus a suffix (e.g. `-note`).

For instance, given these thesaurus entries:

```json
{
  "id": "pes.nota",
  "value": "de pedibus/metris: nota"
},
{
  "id": "pes.species",
  "value": "de pedibus/metris: species"
},
{
  "id": "pes.schema",
  "value": "pedibus/metris: schema/figura"
},
```

the auxiliary thesaurus might include:

```json
{
  "id": "pes.schema--schema",
  "value": "schema-figura"
}
```

where each entry has an ID=built from contains the ID of the target thesaurus entry (here `pes.schema`), followed by `--` and the ID of the annotation's tag (here `schema`); its value is the label of the annotation's tag (here `schema-figura`).

This way, the editor will know that the thesaurus entry ID `pes.schema` can get an annotation with ID=`schema`, labeled `schema-figura` in the UI. This entry ID will have an additional button in the list of selected IDs, allowing you to edit its annotations. When clicked, the annotations will be all those entries whose ID starts with `pes.schema--` in the auxiliary thesaurus (if any); so here you will be able to enter some text content in a text box labeled `schema-figura`.
