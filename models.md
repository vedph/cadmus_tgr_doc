# TGR Models

Models for Tradizione Grammaticale Romana.

## Items

In this project there are just 2 types of items: text with layers, and manuscripts.

### Text

Text with layers.

- [token-based text part](https://github.com/vedph/cadmus_core/wiki/General-Parts#token-text)\* (`TokenTextPart`)
- [note part](https://github.com/vedph/cadmus_core/wiki/General-Parts#note) for generic notes (`NotePart`)
- [note part](https://github.com/vedph/cadmus_core/wiki/General-Parts#note) for translation, with `role`=`transl` and `tag`=language (`NotePart`).

Layer parts:

- [apparatus layer](https://github.com/vedph/cadmus_core/wiki/Philology-Parts#apparatus-model) (`ApparatusLayerFragment`)
- [literary quotations layer] (`QuotationsLayerFragment`) for _apparatus fontium et locorum classicorum_.
- transcriptions layer (`TranscrLayerFragment`), used with different roles for:
  - paleographic transcriptions
  - glosses
  - paratext
- interpolations layer (`InterpLayerFragment`)
- linguistic tags layer (`LingTagsLayerFragment`)

### Manuscript

- [historical date](https://github.com/vedph/cadmus_core/wiki/General-Parts#historical-date-model)\*
- [generic bibliography part](https://github.com/vedph/cadmus_core/wiki/General-Parts#bibliography)
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
  - work (`string`, thesaurus): author and work; this can be picked from a thesaurus, or manually written.
  - location (`string`)
  - title (`string`)
  - incipit\* (`string`)
  - explicit\* (`string`)
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
  - `language`\* (`string`, [ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3), thesaurus)
  - `note` (`string`)
  - `handId` (`string`)

### QuotationsLayerFragment

Literary quotations.

- entries (`VarQuotationEntry[]`): quotations with variants:
  - `tag`\* (`string`, thesaurus): this is used to classify the quotation, e.g. as one quotation included for comparison purposes.
  - `work`\* (`string`, thesaurus): author and work.
  - `location`\* (`string`): location in the work (book, chapter, etc.)
  - variants (`QuotationVariant[]`): variant readings:
    - `lemma`\* (`string`): the lemma
    - `type`\* (`string`, thesaurus, equal to that of the apparatus entry)
    - `value`\* (`string`): the value (zero for deletions)
    - `sources`\* (`ReadingSource[]`):
      - `type` (`string`, thesaurus): type of source: direct tradition from cited authors, philologists, etc.
      - `work`\* (`string`, thesaurus): author and work.
      - `location`\* (`string`): location in the work (book, chapter, etc.)

As you can see, the variants listed for the quotations are located only for human readers via `lemma`.

### InterpLayerFragment

Humanistic interpolations.

- `entries` (`InterpolationEntry[]`):
  - `type`\* (string, thesaurus)
  - `language`\* (`string`, [ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3), thesaurus)
  - `value`\* (`string`)
  - `tag` (`string`, thesaurus)
  - `groupId` (`string`)
  - `note` (`string`)
  - `sources` (`ReadingSource[]`)
    - `witness`\* (`string`, thesaurus)
    - `handId` (`string`)
  - `quotations` (`VarQuotationEntry[]`)

### TranscrLayerFragment

- entries: `TranscriptionEntry[]`:
  - type\* (`string`, thesaurus)
  - role\* (`string`, thesaurus): "paleographic transcription", "gloss", "paratext".
  - language (`string`, [ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3), thesaurus)
  - value\* (`string`)
  - tag (`string`, thesaurus)
  - groupId (`string`)
  - sources (`MsReadingSource[]`):
    - witness\* (`string`, thesaurus)
    - handId (`string`)
  - note (`string`)

### LingTagsLayerFragment

Tag: `fr.it.vedph.tgr.ling-tags`.

- `forms` (`LingTaggedForm[]`):
  - `lemmata` (`string[]`: optional normalized text forms
  - `dubious` (`boolean` (Thesaurus dubii sermonis)
  - `note` (`string`)
  - `tags` (`AnnotatedTag[]`):
    - `value`\* (`string`)
    - `notes` (`TaggedNote[]`):
      - `tag`\* (`string`)
      - `note`\* (`string`)

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
