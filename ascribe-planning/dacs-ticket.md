# Abstract

DACS, an influential copyright society that runs marketplaces to sell images online is interested in integrating with
ascribe. This ticket outlines the agreements made between DACS and BigchainDB.


# Requirements

This section briefly outlines the requirements of this ticket.


## Database registry

- BigchainDB supplies DACS with an API to register artworks and their meta data (already exists - SPOOL Ownership API)


The following flow is supposed to be implemented:

1. An Artist registers an account with ascribe.io
2. An Artist uploads a work
3. An Artist consigns a **Piece** to DACS under a **custom-contract**
4. A Buyer buys a **Piece**
5. DACS loans the Piece to the Buyer under a **custom-contract**


## Clear Licensing

- When images are bought and downloaded, **the license will be attached to the file with clear usage rights**


## Whereonthe.net integration

- When users search for a DACS-licensed image on whereonthe.net, a fingerprint of the image is generated using
  **image-match**. This fingerprint is used to compare the image to ascribed pieces. If a match is found, the piece's
  meta data is displayed on whereonthe.net


# Challenges

- DACS wants to integrate with the SPOOL Ownership API, which is soon to be deprecated
- DACS *probably* wants deep integration into the SPOOL API (meaning creating user accounts and so on), we do not
  support that
- The flow requires a Piece to be consigned with a custom contract attached, something we do not support as of this point
- The flow requires a download to include the license as a .pdf file, something we do not support
- whereonthe.net integration requires that all images uploaded to ascribe are image matched
- whereonthe.net integration requires an endpoint from ascribe to lookup pictures with a corresponding fingerprint,
  something we do not support
- image-match only supports Python2.7. In the near future (3-4 weeks) SPOOL will only support Python3 to run with
  BigchainDB. This means image-match will need to be ported to Python3. In order to port a Python library to Python3, a
  test coverage of 80% is necessary. Image-match has 0% test coverage (it doesn't have tests).


# Tasks

- [ ] Port Pyspool to Python3
- [ ] Port SPOOL to Python3
- [ ] Port Image-match to Python3
- [ ] Retroactively "Image-match" all ascribed works
- [ ] Implement an endpoint for SPOOL to look for similar Pieces with an image-match hash (returns a Piece)
- [ ] Implement custom-contract consign for Pieces in SPOOL and SPOOL protocol
- [ ] Implement a custom-contract loan for Piece (not sure if we support it already)
- [ ] Fix whereonthe.net (yep, it's broken)
- [ ] Implement image-match for whereonthe.net which is Python2.7
- [ ] (Tentative) Port whereonthe.net to Python3 or use image-match through an API
- [ ] Design and Implement cards for whereonthe.net
- [ ] Support DACS integrating with the SPOOL Ownership API
