# Identity Document Verification System (KYC Prototype)

An end-to-end identity verification pipeline combining OCR field extraction, face matching, and digital tampering detection — built as an advanced portfolio project simulating a real KYC (Know Your Customer) workflow used in banking and telecom customer onboarding.

## Architecture
Document image → OCR field extraction (Tesseract + regex) → Face matching against a selfie (face_recognition, 128-dimension face encodings) → Tampering detection (Error Level Analysis) → Decision engine (auto-approved / needs review / rejected) → served via a live FastAPI endpoint.

## Dataset
Uses Labeled Faces in the Wild (LFW), a real, publicly available academic face dataset, rather than synthetic images — ensuring face matching was validated on genuine human faces.

## Results

| Scenario | OCR Confidence | Face Distance | Match | Decision |
|---|---|---|---|---|
| Genuine case (same person) | 1.00 | 0.535 | True | auto_approved |
| Fraud attempt (different person) | 1.00 | 0.974 | False | rejected |

The system correctly distinguishes a genuine identity match from a fraud attempt, with clear, human-readable reasoning attached to each decision — not just a black-box yes/no.

## Key Design Decisions
- **Distance-based face matching** using the standard threshold (<0.6) for the face_recognition library, validated against both a true-match and a true-mismatch case, not just one favorable example.
- **Error Level Analysis** for tamper detection — a real digital forensics technique, not a placeholder check.
- **Decision engine with reasons + audit timestamp**, mirroring real-world human-in-the-loop review workflows rather than fully automated black-box decisions.

## How to Run
1. Open the `.ipynb` notebook in Google Colab
2. Run all cells in order (Runtime → Run all)
3. Test with the included sample images, or upload your own ID photo + selfie

## Challenges & Debugging
- Fixed an image-scaling bug that caused face photos to render completely black (source data was 0–1 scale, not 0–255)
- Fixed a face-detection failure caused by testing on a full ID card instead of a cropped face region
- Fixed an inverted similarity/distance threshold that was incorrectly rejecting genuine matches
- Worked around repeated Colab/ngrok infrastructure instability by testing the API via localhost within the same session

## Limitations & Next Steps
- ID document is a constructed layout (real face + generated fields), not a genuine national ID format
- Field extraction is regex-based; a layout-aware model (e.g. LayoutLM) would generalize better across real-world document templates
- Tested on two illustrative cases; a production system would need evaluation across a larger, more diverse sample

## Tech Stack
Python · OpenCV · Tesseract OCR · face_recognition · scikit-learn (LFW dataset) · FastAPI · Pandas · Google Colab
