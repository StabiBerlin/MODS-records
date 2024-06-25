# DIVERGE mods records
This is the repo containing MODS files for the corpus of the Divergent Discourses project. 

## Changes

- [x] `script="{}"` is unnecessary
- [ ] `<place>`
  - [x] nested place elements are not valid see [originInfo](https://www.loc.gov/standards/mods/userguide/origininfo.html)
  
  ```xml
  <place>
    <placeTerm type="text" authority="marc" lang="tib" script="Tibt">ཀ་ཏ་</placeTerm>
    <placeTerm type="text" authority="marc" lang="tib" script="Latn">ka ta</placeTerm>
    <placeTerm type="text" authority="marc" lang="eng">Calcutta</placeTerm>
    <place>
      <placeTerm type="text" authority="marc" lang="eng">IND</placeTerm>
        <place>
          <placeTerm type="text" authority="marc" lang="eng">West Bengal</placeTerm>
            <place>
                <placeTerm type="text" authority="marc" lang="tib" script="Tibt">ཀ་ལིཀ་ཏ</placeTerm>
                    <placeTerm type="text" authority="marc" lang="tib" script="Latn">ka ta</placeTerm>
                    <placeTerm type="text" authority="marc" lang="eng">Calcutta</placeTerm>
                </place>
            </place>
        </place>
    </place>
  </place>
  ```

  produces the following error:

  ```shell
  Schema: https://www.loc.gov/standards/mods/v3/mods-3-8.xsd
  Engine name: Xerces
  Severity: error
  Problem ID: cvc-complex-type.2.4.a
  Description: Invalid content was found starting with element '{"http://www.loc.gov/mods/v3":place}'. One of '{"http://www.loc.gov/mods/v3":placeTerm, "http://www.loc.gov/mods/v3":placeIdentifier, "http://www.loc.gov/mods/v3":cartographics}' is expected.
  Start location: 23:14
  End location: 23:19
  URL: http://www.w3.org/TR/xmlschema-1/#cvc-complex-type
  ```

  - [ ] add `<placeIdentifier>` to properly identify geo entity see #4
  - [ ] unclear what this structure is trying to say `ka ta` and `calcutta` appear twice which seems superfluous. `IND` does not appear to be a geographical subsection of `Calcutta`. If `་ལིཀ་ཏ` is a relevant alternative `placeTerm` it should appear without nesting next to the others.

  ```xml
  <place>
    <placeTerm type="text" authority="marc" lang="tib">ཀ་ཏ་</placeTerm>
    <placeTerm type="text" authority="marc" lang="tib" transliteration="ISO">ka ta</placeTerm>
    <placeTerm type="text" authority="marc" lang="eng">Calcutta</placeTerm>
    <placeTerm type="text" authority="marc" lang="eng">West Bengal</placeTerm>
    <placeTerm type="text" authority="marc" lang="tib">ཀ་ལིཀ་ཏ</placeTerm>
    <placeTerm type="text" authority="marc" lang="tib" transliteration="ISO">ka ta</placeTerm>
    <placeTerm type="text" authority="marc" lang="eng">Calcutta</placeTerm>
    <placeTerm type="text" authority="marc" lang="eng">IND</placeTerm>
  </place>
  ```

- [x] Missing `@transliteration` attribute on elements with with `tib-Latn` or `chi-Latn`
- [x] empty `<originInfo/>` is invalid see #2
- [x] inconsistent use of `@lang` / `@xml:lang` attribute. The language of the record is not defined, in `<languageOfCataloging>`. Consequently elements like `<name>`, or `@type="translated"` are without language data. see #3
- [x] `<recordInfo>` is missing.
- [x] `<mods>` root is missing desirable attributes `@version`, `@schemaLocation`, `@xmlns:xsi`:

  ```shell
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
  version="3.8" 
  xsi:schemaLocation="http://www.loc.gov/mods/v3 http://www.loc.gov/standards/mods/v3/mods-3-8.xsd"
  ```