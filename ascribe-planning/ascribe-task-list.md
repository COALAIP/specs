# Ascribe 2.0 Task List

This document enumerates all tasks that need to be done to implement ascribe 2.0.


## IPDB tasks

- (COALA IP) Complete implementation of py-ipld
- (COALA IP) Implement ipld in BigchainDB as a configurable option
- (COALA IP) Implement divisible assets in BigchainDB
- Live running instance of IPDB


## COALA IP

- Define ontology in RDF-schema
- Implement ledger-agnostic COALA IP python library
- Implement a ledger-plugin for COALA IP python library for BigchainDB


## Ascribe API

- From COALA IP specification, derive and document REST API on Apiary
    - OAuth system (signing transactions in a client?)
    - Party endpoint
    - Creation endpoint
    - Manifestation endpoint
        - Manifestation file upload
    - Right endpoint
        - Contract file upload
        - Transfer a Right endpoint
- Implement endpoints according to REST API documentation
    - User endpoints
        - Sign up
        - Sign in
        - Logout
        - Reset password
        - Change password
        - API token management
    - Repurpose spoolio email django app and email templates
    - Registration endpoints
        - Register a Creation
        - Register and upload Manifestations for a Creation
            - Thumbnail generation for Manifestations
            - Automatic conversion of audio and video files
        - Create and upload custom contracts for a Right
        - Upload custom contracts for a Right-transfer


## Ascribe Frontend

- Port most functionality (basically adjust as much as possible)
    - Uploading process
    - Generic and high level components
- Design a Creation list and single view (Searchable, paginated, filterable?, sortable?)
- Design Manifestation manage view for a Creation
- Design Contract management
- (Maybe) design a timeline of events that happpened


## DACS

- Image match and "fingerprint" all newly registered Manifestation
- Design and implement DACS/Ascribe card for whereonthe.net


## bokk

- Public collection page for their admin account


## Optional/Later Tasks

Note: If possible we don't do these tasks, but we keep them in the back of our heads while building.

- Easy to extend, manage and change KPI system (not custom-built!)
- Transfer a Creation endpoint (Copyright transfer)
- Assertion endpoint
- RightsConflict endpoint
- Certificate of Authenticity
