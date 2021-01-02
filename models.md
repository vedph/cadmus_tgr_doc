# TGR Models

Models for _Tradizione Grammaticale Romana_. You can also have a glance at the [graphical overview](overview.md).

## Items

In this project there are just 2 types of items: text with layers, and manuscripts.

### Text

Text with layers.

- [token-based text part](https://github.com/vedph/cadmus_doc/blob/master/web/help/general-parts.md#token-text)\* (`TokenTextPart`)
- [note part](https://github.com/vedph/cadmus_doc/blob/master/web/help/general-parts.md#note) for generic notes (`NotePart`)
- [note part](https://github.com/vedph/cadmus_doc/blob/master/web/help/general-parts.md#note) for translation, with `role`=`transl` and `tag`=language (`NotePart`).

Layer parts:

- [apparatus layer](https://github.com/vedph/cadmus_doc/blob/master/web/help/philology-parts.md#apparatus) (`ApparatusLayerFragment`)
- [quotations layer](#varquotationslayerfragment) (`VarQuotationsLayerFragment`) for _apparatus fontium et locorum classicorum_.
- [interpolations layer](#interpolationslayerfragment) (`InterpolationsLayerFragment`), used with different roles for:
  - paleographic transcriptions
  - glosses
  - paratext
  - humanistic interpolations
- [linguistic tags layer](#lingtagslayerfragment) (`LingTagsLayerFragment`)

### Manuscript

- [historical date](https://github.com/vedph/cadmus_doc/blob/master/web/help/general-parts.md#historical-date)\*
- [generic bibliography part](https://github.com/vedph/cadmus_doc/blob/master/web/help/general-parts.md#bibliography)
- [ms signature(s)](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-signatures-part.md)\* (Itinera)
- [ms place](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-place-part.md)\* (Itinera; place of origin; the places of provenance are listed in the ms history).
- [ms contents](#mscontentspart)\* (`MsContentPart`)
- [ms units](#msunitspart)\* (`MsUnitsPart`)
- [ms scripts](#msscriptspart) (`MsScriptsPart`)
- [ms formal features](#msformalfeaturespart) (`MsFormalFeaturesPart`)
- [ms ornamentations](#msornamentspart) (`MsOrnamentsPart`)
- [ms history](#mshistorypart)\* (`MsHistoryPart`)

## Parts

### MsSignaturesPart

See [Itinera](https://github.com/vedph/cadmus_itinera_doc/blob/master/models.md#mssignaturespart).

### MsPlacePart

See [Itinera](https://github.com/vedph/cadmus_itinera_doc/blob/master/models.md#msplacepart).

### MsContentsPart

- `contents` (`MsContent[]`):
  - `start`\* (`MsLocation`)
  - `end`\* (`MsLocation`)
  - `work` (`string`, hierarchical thesaurus: `author-works`): author and work; this can be picked from a thesaurus, or manually written.
  - `location` (`string`)
  - `title` (`string`)
  - `incipit`\* (`string`)
  - `explicit`\* (`string`)
  - `editions` (`DocReference[]`): references to modern editions.
  - `note` (`string`)

### MsUnitsPart

Codicological description.

- `units`: `MsUnit[]+` (codicological units):
  - `start`\*: `MsLocation`
  - `end`\*: `MsLocation`
  - `groupId` (`string`)
  - `groupOrdinal` (`number`)
  - `palimpsests` (`MsPalimpsest[]`): the sheet(s) which are palimpsests inside this unit:
    - `locations` (`MsLocation[]`): 1 or more sheets in the unit's range.
    - `date` (`HistoricalDate`)
    - `note` (`string`)
  - `material`\* (`string`, thesaurus: `ms-materials`)
  - `sheetCount`\* (`int`)
  - `guardSheetCount`\* (`int`)
  - `guardSheets` (`MsGuardSheet[]`):
    - `isBack` (`boolean`)
    - `material` (`string`, thesaurus: `ms-materials`)
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
    - `manner` of execution (`string`, thesaurus: `ms-ruling-manners@en`)
    - `system` (`string`, thesaurus: `ms-ruling-systems@en`)
    - `type` (`string`)
    - `description` (`string`)
  - `watermarks` (`MsWatermark[]`)
  - conservation `state` (`string`)
  - `binding` (`string`)

### MsScriptsPart

- `scripts` (`MsScript[]`):
  - `role`\* (`string`, thesaurus: `ms-script-roles`); e.g. mano principale, secondaria, scriptio superior, scriptio inferior...
  - `language`\* (`string` [ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3), thesaurus: `ms-languages`)
  - `type` (`string`, hierarchical thesaurus: `ms-script-types`)
  - `hands` (`MsHand[]`+):
    - `id`\* (`string`)
    - `date` (`HistoricalDate`)
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
  - `type`\* (`string`, thesaurus: `ms-ornament-types`)
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
  - `language`\* (`string`, [ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3), thesaurus: `ms-languages`)
  - `text`\* (`string`)
  - `note` (`string`)
  - `handId` (`string`)
- `annotations` and corrections (`MsAnnotation[]`):
  - `language`\* (`string`, [ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3), thesaurus: `ms-languages`)
  - `note` (`string`)
  - `handId` (`string`)

### VarQuotationsLayerFragment

Literary quotations.

- `quotations` (`VarQuotation[]`): quotations with variants:
  - `tag` (`string`, thesaurus: `quotation-tags`)
  - `authority`\* (`string`, thesaurus: `quotation-authorities`): the authority type (grammatical/linguistic)
  - `work`\* (`string`, hierarchical thesaurus: `author-works`): author and work.
  - `location`\* (`string`): location in the work (book, chapter, etc.)
  - `parallels` (`QuotationParallel[]`): further occurrences of the same quotation in other grammatical works:
    - `tag` (`string`, thesaurus: `quotation-tags`)
    - `work`\* (`string`, thesaurus: `author-works`): author and work.
    - `location`\* (`string`): location in the work (book, chapter, etc.)
  - `variants` (`QuotationVariant[]`): variant readings:
    - `lemma`\* (`string`): the lemma
    - `type`\* (`string`, enumerated type equal to that of the apparatus entry type)
    - `value`\* (`string`): the value (zero for deletions)
    - `witnesses` (`AnnotatedValue[]`):
      - `value`\* (`string`): e.g. "O"
      - `note` (`string`): e.g. "manus altera"
    - `authors` (`LocAnnotatedValue[]`):
      - `tag` (`string`): any optional classification for the author (e.g. ancient vs modern).
      - `value`\* (`string`): e.g. "Wilamowitz" or "Serv.Aen.": either freely typed, or picked from a thesaurus (`author-works`).
      - `location` (`string`): e.g. "Kleine Schriften, p.12" or "12, 587"
      - `note` (`string`): e.g. "exempli gratia"

As you can see, the variants listed for the quotations are located only for human readers via `lemma`.

### InterpolationsLayerFragment

Interpolations and transcriptions.

- `interpolations` (`Interpolation[]`):
  - `type`\* (string, enumeration equal to that of the apparatus entry type)
  - `role`\* (`string`, thesaurus: `interpolation-roles`): "paleographic transcription", "gloss", "paratext".
  - `tag` (`string`, thesaurus: `interpolation-tags`)
  - `languages`\* (`string[]`, [ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3), thesaurus: `interpolation-languages`)
  - `value`\* (`string`)
  - `groupId` (`string`)
  - `note` (`string`)
  - `sources` (`ReadingSource[]`)
    - `witness`\* (`string`, thesaurus: `apparatus-witnesses`)
    - `handId` (`string`)
  - `quotations` (`VarQuotation[]`)

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
