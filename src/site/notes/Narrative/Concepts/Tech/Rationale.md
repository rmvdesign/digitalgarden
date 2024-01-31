---
{"aliases":null,"tags":null,"dg-publish":true,"permalink":"/narrative/concepts/tech/rationale/","dgPassFrontmatter":true}
---

**Rationale** refers to the comprehensive set of descriptors that accompany every file or data sequence stored within an Operation Controlled SLC. This metadata is integral to an SLC’s ability to manage and retrieve data efficiently from the magnetic tape matrix. Rationale is characterised by several attributes:

### Attributes

- **Title (T):** A descriptive name for the file, including the kind identity prepended.
- **Start Location (SL):** The physical origin point on the tape where a file's data sequence commences.
- **Index Pointing (IP):** The referencing system that allows the SLC’s tape heads to navigate to the start location of a file.
- **Makeup (C):** Describes the internal structure of the file, such as the order of words and the balance of data types it contains. One record per structural block.
- **Word Lengths (A):** The measure of how data within the file is segmented into logical 'words,' which can be of variable length. Use hyphens to distinguish between blocks defined in C records, and colons to distinguish between words within the same C record.
- **Type of File (FT):** Categorises the file as digital, analogue, or hybrid, indicating what sort of data it encompasses and how the tape encodes it.
- **Kind of File (FK):** The equivalent of file format or MIME type, specifying the data format and intended application of the file.
- **Author (OR):** The originator or entity responsible for the creation or assembly of the data sequence. Multiple records allowed.
- **Dates (RD):** Relevant dates for the file. A new entry is added when the file is modified. The first RD entry is the file creation date. Additional TX records can be used as change descriptors.
- **Arbitrary Data (TX):** Additional, user-defined information intended to provide further context or instruction regarding the file. Commonly includes a user-readable file description.

## Function

Rationale plays a pivotal role in the daily workings of an SLC. When a user or system function calls for a file, the Operation Control system, which serves as the SLC’s operating system, deciphers the Rationale to understand how best to process that request. Given that SLCs manipulate logical 'words' rather than fixed-byte structures, Rationale provides the necessary tape coordinates and structure guidance to expedite the Lift-and-Drop operations that characterise data processing on these machines.

## Usage

Detailed Rationale metadata is typically automatically generated upon the creation or import of data into an SLC, though users can modify or append Arbitrary Data at their discretion. The Rationale not only assists the system in locating and managing files but is also invaluable for system diagnostics, security protocols, and data archiving.

When a user queries the system for files—through a method analogous to the 'Echo Queries' used to probe the interspatial PlaNet data vaults—the Rationale metadata ensures that the file presented matches the user's specifications, even amidst millions of words intricately recorded on the tape's labyrinthine layers.

SLCs’ reliance on Rationale metadata underscores the importance of rich contextual information in managing complex data. It's not simply an organisational tool; Rationale encapsulates the essence of every file, offering insights that transcend mere location and type—bridge the digital and analogue realms within the semi-linear universe of data processing.

## Sample Table

| Rationale Attribute | Value |
| ---- | ---- |
| T | TEXT:spec-rationale |
| SL | 002344-002352 |
| IP | 2A-13C-8B-FF4 |
| A | 12-45:33:12:5-20 |
| C | Header |
| C | Text-Universal |
| C | Footer-Signed |
| RD | 1-0035.23 |
| TX | File created, initial spec added. |
| RD | 1-0061.44 |
| TX | Minor revisions. |
| FT | Digital |
| FK | `TEXT`:Text Document |
| OR | Woodrow Terminal Corporation |
| OR | Principal Tape Scribe: Sx. Catheryne Sterling |
| TX | Notes on Rationale file format |
| TX | Consultation with Theotech's SLC division |
