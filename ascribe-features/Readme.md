# Ascribe.io feature list

This is a list of all features currently in production for ascribe.io.


## General features
- Email sent to users for various actions
- Browser title automatically set to page the user is viewing currently
- Responsive web design: Website usable with mobile, tablet and desktop
- Error reporting to Sentry and Papertrail
- KPIs based on back end logs
- Elaborate permissions system to show and hide features based on:
    - user's role
    - Piece or editon
    - Whitelabel page


## Whitelabelability

- Favicon
- Styling for the whole front end
- Landing page
- Registration form
- Further detail fields
- Extra pages (e.g. "Send new contract" in Ikono)
- Filters for collection page
- Emails sent from the back end
    - Styling
    - Content
    - Additional emails
- Hiding and showing of features/elements through permission system (ACLs)
- Footer (Styling + Content)


## User

- Send all types of emails to user
- Sign up
- Sign in
- Logout
- Reset password
- Notifications for pending consign requests
- Emails and UI in multiple languages (English and partially French)
- Editable user name
- Manageable list of API tokens (with custom name)
- Manageable list of Webhooks bound to actions: Share, loan, transfer, consign
- Cryptowallet information: Bitcoin public key, root address of HD bitcoin wallet


## Collection

- Paginated & filterable & sortable & searchable collection view for pieces
    - Expandable (and paginated) tables for editions of a piece
    - Bulk actionable/selectable table items for editions (+ "Select all editions")
        - Share
        - Transfer
        - Consign
        - Loan
        - Delete
    - Filter pieces by:
        - Transfer
        - Consign
        - Create editions
    - Sort pieces by:
        - Artist name
        - Work title
    - Full text search of pieces via string
        - If empty search: Hint user to search for something different or "Clear search"
    - Edition creation dialog in the collection view


## Registration

- Direct upload to AWS S3 from the browser
    - Upload of multiple files
    - Live progress update for upload
    - Drag and Drop for multiple files
    - Thumbnail for upload
    - Display of uploading file name
    - Cancelable uploading process (stoping it while uploading)
    - Resetable uploading process (stoping upload after successfully uploading)
    - If enabled in settings: Hashing and uploading of .txt file of file instead of file in uploader
- Definition and async creation of editions in registration
- Automatic conversion and thumbnail creation of media files after upload:
    - Generate thumbnails for GIFs (to GIF), JPGs, PNGs, BMPs (to JPG), TIFs (to JPG) and creation of thumbnail for all
      supported video formats
    - Conversion from OGG, WMA, MP3, WAV to web supported formats
    - Conversion from MOV, MPG, MPEG, M4V, OGV, AVI, WEBM, MP4 to web supported formats
    - If automatic thumbnail creation doesn't support file format, allowance for definition of a representative image
        - Upload of single file
        - Resetable uploading process



## Piece and Edition Page
- Share to Twitter and Facebook
    - Render a preview for Twitter and Faceook crawlers
- Direct upload of multiple files to AWS S3 from the browser (additional files)
    - Deletable files
    - Downloadable files
        - File downloaded has name: [Piece/edition title-artistname.filetype]
- Preview for most used media files
    - Preview for large image files via shmui
    - Preview for audio files
    - Preview for video files
- Embeddable script for audio and video files
- Actions for piece or edition
    - Share: Piece or edition shows up in sharee's collection
    - Transfer: Edition is owned by transferee
    - Consign: Edition is managed by consignee
    - Loan: Piece or edition is publicly displayable by loanee
    - Delete: Piece or edition is deleted from SQL database (not from the BTC blockchain)
    - Expandable information for all actions
- Personal and private notes
- Generic further details fields
    - Artist contact info
    - On field freeze: Links and emails become clickable links
    - Display instructions
    - Technology Details
- Textarea fields
    - Actions: save and cancel
    - Upon typing a new line, textarea expands to show all text
- SPOOL details: Artwork ID, Hash of Artwork, Owned by SPOOL address
- Loan History
    - Shows when edition loaned under a contract (contract link provided)


## Edition page specifics

- Provenance History
- Consignment History
    - Shows when edition consigned under a contract (contract link provided)
- Certificate of Authenticity
    - Asynch creation of certificates
    - Preview in browser
    - Downloadable as PDF
    - Verification of COA crypto signature on extra "verify" page


## Piece and Edition action modes

- All actions + modes: Personal message is definable
- Share via email: email sent to sharee, piece or edition shows up in collection
- Transfer: email sent to transferee, edition shows up in collection
- Loan:
    - All modes: start and end loan data are selectable
    - Normal: email sent to loanee, piece or edition shows up in collection
    - Contract: loaner accepts contract uploaded by loanee (contract previews upon typing of email (is also
      downloadable)), email sent to loanee, piece or edition shows up in collection
    - Pending: Loanee can accept/reject loan of loaner, loaner gets email
- Consignment:
    - Normal: email sent to consignee, edition shows up in collection
    - Contract: consigner accepts contract uploaded by consignee, email sent to consignee, edition shows up in
      collection
    - Pending: Consignee can accept/reject consignment of consigner, consigner gets email
    - Unconsign: Consignee can back-consign consignment to consigner, consigner gets email


## Whitelabel custom features

- Custom landing page


### cc.ascribe.io


#### Registration

- Choose between six CreativeCommons licenses


#### Collection

- Display of chosen CC license in collection piece items


### cyland.ascribe.io


#### User

- Manageable list of uploaded contracts for admin


#### Collection

- Custom filter function for pieces:
    - submitted
    - loaned


#### Registration and Piece Pages

- Domain-specific further detail fields
- Streamlined registration and custom-contract-loan process for pieces


### ikonotv.ascribe.io


#### User

- Invite only members
    - Acceptance of a custom contract, uploaded through admin ("SEND NEW CONTRACT")
    - Ability of definition of appending to the contract
- Whitelabeled emails for contract acceptance
- Manageable list of uploaded contracts for admin
- Contract review page (accept or reject, respective emails are sent to contracter)
    - PDF preview for contract
    - Contract downloadable
    - Selection option for copyright societies


#### Registration and Piece Pages

- Domain-specific further detail fields
- Streamlined registration and custom-contract-loan process for pieces
- Alert notice when registration process wasn't completed


### Consignment-Wallet (23vivi, lumenus, artcity, etc.)


#### User

- Whitelabeled emails for:
    - Transfer
    - Consign
- Manageable list of uploaded contracts for admin


#### Registration and Piece Pages

- Streamlined registration and custom-contract-loan process for editions
- Domain-specific further detail fields


#### Collection

- Custom filter function for pieces:
    - submitted
    - loaned
