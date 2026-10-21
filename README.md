# TryHackMe - The Letter | OSINT Writeup

**Room:** https://tryhackme.com/room/letter
**Category:** OSINT / Historical Research / Image Forensics
**Difficulty:** Easy
**Status:** Completed 100% - 20 Oct 2026

### Objective
> Use the clues provided in the zip file to uncover the full name and age of the person mentioned in the note.
> Flag Format: `THM{Name_Surname_age}`

### Files Provided
- `letter.png` - Battered Lettre Verte envelope
- `Newspaper_clipping.png` - Torn clipping from L'Ouest-Éclair
- `Note.txt` - Handwritten note in French

### Investigation Walkthrough

#### Step 1: Date the Newspaper
Secondary headlines visible after enhancement:
- `Amundsen a-t-il atteint le pôle Nord ?`
- `M. Herriot se déclare solidaire de M. Painlevé`

OSINT pivot: Roald Amundsen's North Pole flight attempt = May 21, 1925. Painlevé government = April-Nov 1925.
=> **Late May 1925.**

#### Step 2: Identify the Disaster
Search in French: `catastrophe Finistère canots de sauvetage mai 1925`
Result: **Penmarc'h Lifeboat Disaster - May 23, 1925.** Two SNSM rescue boats capsized, 27 victims.

#### Step 3: Postal Code (Q1) - The Trap
The envelope to Édouard G. / SNSM looks like `53432` at first glance, but:
- 53 = Mayenne = landlocked department, no SNSM station
- Real location = Penmarc'h, Finistère (29)

**Answer 1: 29760**

*Forensics note:* Used PIL to enhance blue ballpoint:
```python
from PIL import Image, ImageOps, ImageEnhance
img = Image.open('letter.png')
img = ImageOps.autocontrast(img)
ImageEnhance.Contrast(img).enhance(3.0)

Step 4:

Person in the Note (Q2)Note says: "Ton arrière-grand-père n'avait même pas l'âge de passer le permis... Le benjamin de l'équipe"
=> Youngest of the crew, under 18.Search: sauvetage penmarc'h 1925 benjamin mousse age
Result: Yves-Marie Gourlaouen, mousse (cabin boy), 15 years old.
Flag: THM{Yves-Marie_Gourlaouen_15} 

Tools

GIMP / Python PIL - image forensics on faded ink
Gallica.bnf.frFrench newspaper archives
French keyword Google Dorking

What I Learned

Dating torn documents via secondary headlines
French postal code department logic (29 vs 53)
Pivoting from disaster name to local victim archives
Handling water-damaged evidence
