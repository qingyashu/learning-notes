## CORS (cross-origin resource sharing) Concept
User agent requests resource from different domain than the currently used. E.g: website A contains an image which source is from domain B.
##Security issue
- XMLHTTPRequest and Fetch API only allow requesting same domain resources, unless CORS headers are used. 
- CORS example: 
  - Agent from origin A sends request to domain B. B's response should contains header "Access-Control-Allow-Origin" containing A's domain. 