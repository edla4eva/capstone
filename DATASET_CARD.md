# Dataset Card: Administrative Document Classification and Information Extraction

This dataset is intended to support classification of administrative documents and extraction of relevant information. It covers digital documents and document images, including scanned or photographed pages, email content, and document attachments. Target materials include letters, memos, notices, circulars, emails, and articles.

## Input and output

For scanned or image-based inputs, OCR or an equivalent text-extraction process is used to obtain machine-readable text.

Expected outputs include:

- Document format or genre
- Administrative purpose or action
- Extracted fields, where available: title or heading, subject, date, sender, recipient, reference number, main body text, deadline, and required action

Information that is absent or unreadable is not inferred.

## Classification labels

Classification uses two distinct but related dimensions:

- **Document format or genre:** letter, memo, notice, circular, email, or article.
- **Administrative purpose or action:** request for approval, notice of approval, appointment, recommendation, event invitation, leave request, sick leave, maternity leave, posting, query, petition, report, minutes of meeting, admission letter, or promotion letter.

An `other/unclear` label is available for unsupported or ambiguous examples. When a document reasonably fits more than one purpose, annotations may include multiple labels or designate primary and secondary labels. Urgency is represented separately using dates, deadlines, and time-to-event, rather than as a document type.

## Annotation

Each item is annotated for document format or genre and administrative purpose or action. Where appropriate, annotations also capture title, subject, date, sender, recipient, reference number, body text, required action, deadline, event date, and responsible person or office.

Urgency or time-to-event is derived from reliable dates where possible and marked unknown when information is absent or unclear. Consistent labeling definitions and review procedures are used.

## Data creation and provenance

The dataset is developed using a combination of real-world and synthetic documents. Real examples may include publicly accessible institutional documents and university-related communications that the team is authorized to use. Synthetic examples represent the target categories and introduce variation in wording, structure, format, and layout.

Each record is identified by source type so real and synthetic examples can be monitored separately. Approved real documents are redacted before inclusion in the training dataset. Source and provenance information is recorded for collected items.

## Limitations and responsible use

- The dataset supports document classification and information extraction; extracted information is limited to what is present and legible in the input.
- OCR errors may affect classification and extracted fields for scanned or photographed documents.
- Ambiguous, unsupported, or unreadable examples should be labeled `other/unclear` or marked unknown as appropriate, rather than resolved by guessing.
- Real documents should only be included when the team is authorized to use them and after required redaction.
- Synthetic documents should be tracked separately from real documents when monitoring dataset composition and model performance.
