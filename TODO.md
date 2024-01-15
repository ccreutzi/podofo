### 1.0
- PdfMemDocument: Check the DeviceStream is not empty before doing an incremental update/signing operation
- PdfMemDocument: Prevent Save() operation after signing operation
- PdfMemDocument: Release the device after all objects have been loaded (eg. after a full Save())
- PdfParserObject: Release the device after loading
- Evaluate rename references to "sequential" signing to "async"
- Consider converting protected PdfFontMetrics::GetFaceHandle() to return just FT_Face,
and reference the face with FT_Reference_Face
- Remove PdfEncryptMD5Base::GetMD5Binary and PdfEncryptMD5Base::GetMD5String and use common functions
- PdfField: Evaluate make a virtual getValueObject()
- Evaluate removing PdfObject::Null and PdfVariant::Null and introduce nullptr_t constructor overloads
- PdfMemDocument: Consider removing SetEncrypt(encrypt)
- PdfEncrypt: Consider removing CreateFromEncrypt (shared_ptr in PdfMemDocument could be used now)
- PdfContents: Remove PdfContents::Reset(obj) (keep parameterless) and make constructors private
- Review/refactor PdfColor: it should use new PdfColorSpace and not define a color space
- Review all page import functions to check correct working/improve the code
- Review PdfPageCollection::AppendDocumentPages(),
  PdfPageCollection::InsertDocumentPageAt(), PdfPage::MoveAt()
- PdfCanvas: Add CopyTo facilities, see PdfContents
- Review PdfNameTree
- Check accessibility of PdfEncrypt.h classes, check AESV3 naming
- PdfFilterFactory: Move CreateFilterList somewhere else (PdfFilter), make it private
- Rename NameToColorSpaceRaw/ColorSpaceToNameRaw to something more consistent?
- More enum <-> strings functions and make them public
- Make PdfObjectStream not flate filter by default in PdfMemDocument?
- PdfElement: Optimize, keep dictionary/array pointer. Add GetObjectPtr()
- PdfPageCollection: Add iteration on PdfPage. See PdfAnnotationCollection
- PdfPageCollection::CreatePage() with PdfPageSize or default inferred from doc
- PdfPage: Add GetFields() iteration
- PdfDocument: Add GetAnnotationFields()/GetAllFields() iteration
- Fix PdfFontMetrics handling of symbol encoding
- Check PdfWriter should really update doc trailer when saving.
  Now the new trailer is written but the doc still has the old one
- PdfImage: cache PdfColorSpace
values in the dictionary after signing with SignDocument (???)
- Evaluate move more utf8::next to utf8::unchecked::next
- Add PdfString(string&&) and PdfName(string&&) constructors that
either assume UTF-8 and/or checks for used codepoints
- Added PdfResources::GetResource with enum type
- Add a PdfRect-like class PdfCorners that avoid coordinates normalization
  by default
- PdfToggleButton: Add proper IsChecked/ExportValue handling
- Review PdfPage::SetICCProfile()
- PdfWriter: Check if SetEncrypt() should accept mutable reference instead
- PdfResources: Improve API
- Add version of PdfFont::TryGetSubstituteFont for rendering
  (metrics/widths of loaded font override metrics found on /FontFile)
- Add a fallback to search font on the system for text extraction purposes,
  see #123
- PdfParser: Handle all pdfs in
  https://www.mail-archive.com/podofo-users@lists.sourceforge.net/msg04801.html
- Check/Review doxygen doc

### After 1.0
- Check what do with tools/restore manuals
- Fix/complete handling of text extraction in rotated pages (??? Done?)
- Add method to retrieve shared_ptr from PdfObject, PdfFont (and
  maybe others) to possibly outlive document destruction
- Do more overflow checks using Chromium numerics, which is now
  bundled. See comments in utls::DoesMultiplicationOverflow()
- PdfFontManager: Add font hash to cache descriptor
- Add special SetAppearance for PdfSignature respecting
  "Digital Signature Appearances" document specification
- PdfParser: Handle invalid startxref by rebuilding the index,
  similarly to what pdf.js does
- Add text shaping with Harfbuzz https://github.com/harfbuzz/harfbuzz
- Add fail safe sign/update mechanism, meaning the stream gets trimmed
  to initial length if there's a crash. Not so easy, especially since
  we are now using STL streams and it's not easy to trim files
  without access to native handle and low level I/O operations
- PdfDifferenceEncoding: Rework Adobe Glyph List handling and moving it to private folder
- Option to unfold Unicode ligatures to separate codepoints during encoded -> utf8 conversion
- Option to convert Unicode ligatures <-> separate codepoints when drawing strings/converting to encoded
- Optimize charbuff to not initialize memory, keeping std::string compatibility
- Add backtrace: https://github.com/boostorg/stacktrace

### Ideas:
- PdfFontManager: Consider also statically caching the queries and filepaths.
  Maybe we could also weakly (weak shared pointer) cache metrics instead of fonts
