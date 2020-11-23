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
- [literary quotations layer](https://github.com/vedph/cadmus_core/wiki/Philology-Parts#quotations) (`QuotationsLayerFragment`) for apparatus fontium et locorum classicorum. Parallel quotations entries can be grouped under a common parent quotation using the `tag` property (e.g. tagging the parent as `prisc1` and its children as `prisc1.1`, `prisc1.2`, etc.). This avoids replicating the quotation schema with a potentially recursive nesting and simplifies the schema with a flat list.
- transcriptions layer (`TranscrLayerFragment`), used with different roles for:
  - paleographic transcriptions
  - glosses
  - paratext
- interpolations layer (`MsInterpLayerFragment`)
- linguistic tags layer (`LingTagsLayerFragment`)

### Manuscript

Manuscripts: 2 parts are reused, 8 could be new. Anyway, we should compare this plan with the modeling already in place for the [codicological part](https://github.com/vedph/cadmus_itinera_doc#codicology) of the [Itinera](https://github.com/vedph/cadmus_itinera_doc) project. Having a more or less shared codicological model could be beneficial for both projects, in order to define some common ground and provide more integration potential for any project in the global community.

- [historical date](https://github.com/vedph/cadmus_core/wiki/General-Parts#historical-date-model)\*
- [generic bibliography part](https://github.com/vedph/cadmus_core/wiki/General-Parts#bibliography)\*
- ms shelfmark\*: compare with [Itinera ms signatures](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-signatures-part.md).
- ms country places\*: compare with [Itinera ms place of origin](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-place-part.md) (the places of provenance are listed in the ms history).
- ms contents\*: compare with [Itinera ms contents](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-contents-part.md).
- ms codicological description\*: compare with [Itinera ms material description](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-material-dsc-part.md), [Itinera ms quires](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-quires-part.md), [Itinera ms dimensions](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-dimensions-part.md), [Itinera ms binding](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-binding-part.md), [Itinera ms catchwords](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-catchwords-part.md), [Itinera ms composition](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-composition-part.md).
- ms scripts part\*: compare with [Itinera ms hands](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-hands-part.md) and the corresponding [Itinera persons](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/person-part.md)
- ms formal features?: TODO: define if this is still required or just got merged into `MsScriptsPart`.
- ms decorations: compare with [Itinera ms decorations](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-decorations-part.md)
- ms history\*: compare with [Itinera ms history](https://github.com/vedph/cadmus_itinera_doc/blob/master/help/ms-history-part.md)

## Parts

### MsShelfmarkPart

- libraryId (thesaurus): string\*
- signature: string\*

### CountryPlaces Part

The following model is just a draft for a location based on *modern* countries. Its only required datum is the country; all the other data are optional, and can additionally define a point and/or an approximate square region.

This is a very practical and general-purpose place definition modeled after the [Google geocoding system](https://developers.google.com/maps/documentation/javascript/geocoding).

- `locations`: `CountryLocation[]`+: country-based location:
  - `tag`: string\* (optional thesaurus)
  - `region`: string\* (thesaurus): [ISO 3166-1 alpha 2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)
  - `point`: `LatLonLocation`:
    - `lat`: number (double)
    - `lon`: number (double)
  - `address`: string. The address is a string composed by 1 or more components, separated by commas. Geocoded "addresses" are not just postal addresses, but any way to geographically name a location. For example, when geocoding a point in the city of Chicago, the geocoded point may be labeled as a street address, as the city (Chicago), as its state (Illinois) or as a country (The United States).
  - `plusCode`: string. This refers to a square area using [Open Location Code](https://github.com/google/open-location-code). Probably it has no usage for manuscripts, but could be useful for sites, to provide a precise area without having to describe it exactly with more complex geometries. See [plus codes](https://plus.codes/) for more.
  - `note`: string.

Ancient world modes are different, because there a named place may have several names and several possible locations (or even none). See e.g. 
[Pleiades](pleiades.stoa.org/help/pleiades-data-model).

The point can be geocoded automatically starting from its address (and country), using the Google geocoding service. Anyway, doing it in the app automatically would require a paid subscription to the service (which is paid per usage). We thus have better avoid inserting this functionality in the editor, and rather systematically add it later at once before publishing.

### MsContentsPart

- `contents` (`MsContent[]`):
  - `author`\* (`string`, thesaurus?)
  - `claimedAuthor` (`string`, thesaurus?)
  - `work`\* (`string`)
  - `start` (`MsLocation`)
  - `end` (`MsLocation`)
  - `state` (`string`, thesaurus?)
  - `note` (`string`)
  - `units` (`MsContentUnit[]`):
    - `label` (string)
    - `incipit`\* (string)
    - `explicit`\* (string)

### MsCodexPart

- codicological units: `MsUnit[]+`:
  - start: `MsLocation`\*
  - end: `MsLocation`\*
  - material (thesaurus): string\*
  - leafCount\*: int
  - quires: string
  - leaf/page numbering: string
  - quires numbering: string
  - leafSize: `PhysicalSize`\*:
    - w: `PhysicalDimension`\*:
      - value: float\*
      - unit (thesaurus): string\*
      - incomplete: boolean
    - h\*: as w
    - d: as w, but optional
    - note: string
  - writtenAreaSize: `PhysicalSize`\*
  - ruling: `MsRuling`\*:
    - manner of execution (thesaurus): string
    - system (thesaurus): string
    - type (thesaurus): string
    - description: string
  - watermark: `MsWatermark`\*:
    - value (thesaurus): string\*
    - description: string
  - conservation state: string\*
  - binding: string

### MsScriptsPart

- scripts: `MsScript[]`+:
  - role (thesaurus)\*: string
  - language (thesaurus)\*: string ([ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3))
  - type (thesaurus-h): string
  - hands: `MsHand[]`+:
    - id: string\*
    - datation: `HistoricalDate`
    - start: `MsLocation`\*
    - end: `MsLocation`\*
    - description: string\*
    - letters: `MsHandLetter[]`(0-N):
      - letter: string\*
      - description: string
      - imageId: string
    - abbreviations: string\* (MD)

### MsFormalFeaturesPart

Orthographic/phonetic peculiarities:

- description: string\* (MD)
- hands: `MsHand[]`+

TODO: what is the relationship with hands in scripts??

### MsDecorationsPart

- `decorations` (`MsDecoration[]`):
  - `type`* (`string`, thesaurus)
  - `start`* (`MsLocation`)
  - `end`* (`MsLocation`)
  - `position` (position in page: `string`, thesaurus?) TODO: define: what is exactly?? The relative position in the page layout?
  - `size` (`PhysicalSize`)
  - `description` (`string`, MD, 1000)

### MsHistoryPart

- `provenance`\* (string)
- `history`\* (string, MD, 5000)
- `owners` (`MsOwner[]`):
  - `firstName`
  - `lastName`\*
  - `externalIds` (`string[]`)

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
  - type (thesaurus): string*
  - language (thesaurus): string ([ISO 639-3](https://en.wikipedia.org/wiki/ISO_639-3))
  - value: string*
  - tag (thesaurus): string
  - groupId: string
  - sources: `MsReadingSource[]`+
    - witness (thesaurus): string*
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
