## OVERVIEW

```c++
const uint32		PACKED_SHEET_HEADER = 'PKSH';
const uint32		PACKED_SHEET_VERSION = 5;
// This Version may be used if you want to use the serialVersion() system in loadForm()
const uint32		PACKED_SHEET_VERSION_COMPATIBLE = 0;

// ***************************************************************************
/** This function is used to load values from georges sheet in a quick way.
 * \param sheetFilter a vector of string to filter the sheet in the case you need more than one filter
 * \param packedFilename the name of the file that this function will generate (extension must be "packed_sheets")
 * \param container the map that will be filled by this function
 */
template <class T>
void loadForm (const std::vector<std::string> &sheetFilters, const std::string &packedFilename, std::map<NLMISC::CSheetId, T> &container, bool updatePackedSheet=true, bool errorIfPackedSheetNotGood=true)
```

```c++
// ***************************************************************************
// variant with smart pointers, maintain with function above
template <class T>
void loadForm2(const std::vector<std::string> &sheetFilters, const std::string &packedFilename, std::map<NLMISC::CSheetId, NLMISC::CSmartPtr<T> > &container, bool updatePackedSheet=true, bool errorIfPackedSheetNotGood=true)
```

* packed_sheet
  - PACKED_SHEET_HEADER : uint32
  - PACKED_SHEET_VERSION : uint32
  - PACKED_SHEET_VERSION_COMPATIBLE : uint32
  - dependBlockSize : uint32
  - default
    - dictionnary : vector<std::string> ; XML \<FORM> \<DFN> \<TYP> filenames
    - depSize : uint32
    - : <sheetId, vector<uint32> > [depSize]
  - nbEntries : uint32
  - ver : uint32
  - container : map<NLMISC::CSheetId, T>
