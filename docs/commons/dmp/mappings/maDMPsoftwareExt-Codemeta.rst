
OSTrails Software Extension for maDMPs - Codemeta Mapping
=========================================================


.. page-authors::
    Maria Grazia Santangelo
    Jozefina Sadowska

Introduction
------------

A mapping between CodeMeta format and maDMP fields enables the reuse of
standardised software metadata (e.g. software name, authors, description,
license, repository URL, ...) in Machine-actionable Data Management Plans
(maDMP). This reduces manual data entry, avoids inconsistencies, and improves
metadata quality. It also promotes interoperability between DMP platforms,
software and data repositories, and archival services such as Software
Heritage, supporting software discoverability, citation, preservation, FAIRness
and reproducibility throughout the research lifecycle.

Mapping
-------

.. list-table::
   :header-rows: 1
   :widths: 20 20 60

   * - Software field
     - `Codemeta - V4 <https://codemeta.github.io/terms/>`__
     - Description
   * - Name
     - ``name``
     - Full name of the software
   * - Description
     - ``description``
     - Description of the software
   * - Release date
     - ``datePublished``
     - To indicate a date when a software was published or released. Encoded
       using the relevant ISO 8601 Date
       `compliant string <https://www.w3.org/TR/NOTE-datetime>`__
   * - Application category
     - ``applicationCategory``
     - A main category of the software
   * - Code repository
     - ``codeRepository``
     - URL of the code repository
   * - Registry identifier
     - ``identifier``
     - Identifier for the software
   * - Author
     - ``author``
     - An entity responsible for making the software
   * - Contributor
     - ``contributor``
     - Person or organisation that has contributed to the development of the
       software
   * - Contributor role
     - ``roleName``
     - A role played, performed, or filled by a person or organization
   * - License
     - ``license``
     - Software distribution license
   * - Related identifier
     - ``citeAsID``
     - For specifying related identifiers software, e.g. the SWHID
