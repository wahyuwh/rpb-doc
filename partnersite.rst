Partner Site Creation
=====================

Each partner site in RPB needs to have unique identifier. If RPB is used in its full distributed form, these identifier should be synchronised between institutes who are running their own instance of RPB in order to allow transparent data exchange without conflicts. Good ideas for identifiers are shortcuts abbreviating the institute names that are also very often used as URIs which are by its nature quite stable. Changing the identifier later could be very complicated (however still doable with some effort).

By conventions we use capital letters. It is reasonable to not exceed 6 characters size for the identifier.

Usage of partner site identifier (multi-centre study setups):
- as prefix to ensure the uniqueness of patient pseudonym when patients from multiple sites are stored in one DB  e.g. DD-abc123de
- as prefix for site specific study protocol ID e.g. DD-STR-Demo-2014 

Avoid using "DELETED" string as partner site identifier as we use this keyword in RPB for abstract study sites where patients that should be deleted (withdrawn consent, or mistake enrollment) are migrated.