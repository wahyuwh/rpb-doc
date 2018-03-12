User Documentation
==================

Information about general system usage of all RPB platform components.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   spreadsheet
   

User Creation
=============

It is necessary to create two user accounts for each user. One is the main account in RPB database and the second one is linked account for the access to EDC system (OpenClinica). For internal users we try to follow the usernames already existing in active directory. The user names are case sensitive however RPB application login is automatically converting them into small cases so they should appear as case insensitive to the end user (however [[OpenClinica]] is always case sensitive).

Create a new Study
==================

Define Study in EDC
- Unique Protocol ID: study identifier, use a code which we use to identify the study; first 8 characters (ignoring spaces) of study unique identifier will be prefixed with 'S_' used to generate Study OID
- Site Unique Protocol ID: site study identifier, use RPB partner site unique identifier as prefix: e.g 'DD-' + parent study Unique Protocol ID
- Define a short (2 character), unique trial EDC code (e.g. 'AA', 'XN') and store it as first element in Secondary IDs field, this code is used in RPB naming conventions; optionally use small 'r' or 'p' to denote retrospective or prospective basis of the study in the code (e.g. 'HNp', 'HNr'); check the availability of the code in CTMS
- Create CRFs (at least one [[FormDefinition]] need to be specified), use RPB naming conventions
- Create Event Definitions (at least one [[StudyEventDefinition]] need to be specified), use RPB naming conventions
- Set Study Status: Available

Define Study in CTMS and link it to EDC
- Create RPB study (Administration->Studiesmanagement->Studies) and link (parent) study defined in EDC
- Edit CTMS details for RPB study (CTMS->Studies)
- Specify study tags (EDC Code tag e.g. 'AA', to make it easily possible to check availability of the code for new trials)
- Specify study tumour entities
- Create eCRF field annotations (Administration->Studienmanagement->eCRF Field Annotations)

Partner Site Creation
=====================

Each partner site in RPB needs to have unique identifier. If RPB is used in its full distributed form, these identifier should be synchronised between institutes who are running their own instance of RPB in order to allow transparent data exchange without conflicts. Good ideas for identifiers are shortcuts abbreviating the institute names that are also very often used as URIs which are by its nature quite stable. Changing the identifier later could be very complicated (however still doable with some effort).

By conventions we use capital letters. It is reasonable to not exceed 6 characters size for the identifier.

Usage of partner site identifier (multi-centre study setups):
- as prefix to ensure the uniqueness of patient pseudonym when patients from multiple sites are stored in one DB  e.g. DD-abc123de
- as prefix for site specific study protocol ID e.g. DD-STR-Demo-2014 

Avoid using "DELETED" string as partner site identifier as we use this keyword in RPB for abstract study sites where patients that should be deleted (withdrawn consent, or mistake enrollment) are migrated.