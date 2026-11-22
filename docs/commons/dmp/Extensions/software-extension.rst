

OSTrails Software Extension for maDMPs 
============================================

.. page-authors::
    Maria Grazia Santangelo
    Jozefina Sadowska 
    Tomasz Miksa 

The OSTrails Software Extension for machine-actionable Data Management Plans (maDMPs) defines how software should be represented in maDMPs that comply with the `RDA DMP Common Standard (DCS) for maDMP <https://docs.ostrails.eu/en/latest/commons/dmp/dmp-common-standard.html>`__\ s. The proposed approach represents software as a Dataset and sets ``dataset.type`` to ``"software"``:

.. code-block:: json

   {
     "dataset": [
       {
         "type": "software"
         // ...
       }
     ]
   }

Existing DCS properties are reused wherever they already provide the required semantics, while a limited number of software-specific properties are introduced to capture information that is not currently represented.


Purpose and scope
-----------------

This technical note presents a contribution from the French pilot (Inria, the French National Institute for Research in Digital Science and Technology) to the OSTrails project. The pilot aims to establish a metadata model for describing source code and software in maDMPs, and this note presents the proposed extension to the DMP Common Standard to achieve this goal.

In this context, rather than proposing a new Software Management Plan (SMP), that is, a dedicated model for describing how software is managed throughout a project, we extend the existing maDMP model with additional fields to capture software management information. The main advantage of this approach is that it provides a unified view of the data and software produced and used within a project, avoiding the need to maintain separate management plans while still capturing the specific information required for software management.

Representing software as a Dataset is consistent with the DCS, in which a Dataset is a logical entity that can represent data, software, or publications. The proposed extension adds a small number of properties to the existing DCS Dataset model to describe software. Application category, code repository, contributor, and license are introduced as new fields, while contributor role reuses an existing DCS property with a software-oriented vocabulary. Other properties, including name, description, release date, registry identifier, author, and related identifier, reuse existing DCS properties.

The resulting model keeps software represented within the existing Dataset structure and allows Distributions to be used when specific releases or distribution channels need to be described.


Design approach
---------------

The extension follows four principles:

- Reuse existing DCS fields whenever possible. General metadata such as name, description, release date, identifier, and author are represented using existing DCS fields.

- Reuse established external vocabularies and properties. Where software-specific metadata is not already represented in DCS, the extension reuses properties from established standards such as schema.org, CodeMeta, DataCite and DCAT.

- Maintain interoperability with the existing DCS model. The extension does not introduce a separate top-level software object. Instead, software is represented through the existing Dataset structure, with ``dataset.type = "software"`` identifying the resource as software.

- Exclude properties that are inherently dynamic. Properties whose correct value changes over the lifetime of the software, rather than remaining stable for the software resource as a whole, are not represented as dedicated fields in this version of the extension. An exception is made for Software Heritage Identifiers (SWHIDs), which provide persistent, intrinsic identifiers for source code and specific software revisions. Including SWHIDs supports precise identification, traceability, and reproducibility, even when the software evolves over time.


Mapping of software metadata fields
-----------------------------------

The following table defines the proposed mapping between software properties and DCS fields.

.. list-table::
   :header-rows: 1
   :widths: 10 14 22 8 9 12 11 14

   * - Software field
     - DCS field
     - Description
     - Cardinality
     - Datatype
     - Value constraint / vocabulary
     - Status
     - Source
   * - Name
     - ``dataset.title``
     - Full name of the software
     - 1
     - String
     - As defined by DCS
     - Existing
     - DCS
   * - Description
     - ``dataset.description``
     - Description of the software
     - 0..1
     - String
     - As defined by DCS
     - Existing
     - DCS
   * - Release date
     - ``dataset.issued``
     - To indicate a date when a software was published or released. Encoded using the relevant ISO 8601 Date `compliant string <https://www.w3.org/TR/NOTE-datetime>`__
     - 0..1
     - Date
     - As defined by DCS
     - Existing
     - DCS
   * - Application category
     - ``dataset.application_category``
     - A main category of the software
     - 0..\*
     - String
     - Free value
     - New
     - schema.org (``schema:applicationCategory``; also used by CodeMeta)
   * - Code repository
     - ``dataset.code_repository``
     - URL of the code repository
     - 0..1
     - URL
     - None
     - New
     - schema.org (``schema:codeRepository``; also used by CodeMeta)
   * - Registry identifier
     - ``dataset.dataset_id``
     - Identifier for the software
     - (0..\*)
     - Nested Data Structure
     - As defined by DCS
     - Existing
     - DCS
   * - Author
     - ``dataset.creator``
     - An entity responsible for making the software
     - 0..\*
     - Nested Data Structure
     - As defined by DCS
     - Existing
     - DCS
   * - Contributor
     - ``dataset.contributor``
     - Person or organisation that has contributed to the development of the software
     - 0..\*
     - Nested Data Structure
     - None
     - New (same structure as ``dmp.contributor``)
     - DCS (not linked before from Dataset, but we can add this connection in the extension)
   * - Contributor role
     - ``dataset.contributor.role``
     - A role played, performed, or filled by a person or organization
     - 0..\* per contributor
     - String
     - Suggested values:

       - Design
       - Architecture
       - Debugging
       - Maintenance
       - Coding
       - Documentation
       - Testing
       - Support
       - Management
     - Existing field, new role vocabulary
     - DCS (not linked before from Dataset, but we can add this connection in the extension)
   * - License
     - ``dataset.license``
     - Software distribution license
     - 0..\*
     - URL
     - None
     - New (plain URL list, as in DCAT)
     - DCAT (``dcterms:license``)
   * - Related identifier
     - ``dataset.related_identifier``
     - For specifying related identifiers software, e.g. the SWHID
     - 0..\*
     - Nested Data Structure
     - As defined by DataCite 4.7
     - Existing
     - DCS in alignment with the `DataCite Metadata Schema <https://datacite-metadata-schema.readthedocs.io/en/4.7/properties/relatedidentifier/>`__


Software-specific fields
------------------------



Application category
~~~~~~~~~~~~~~~~~~~~

``dataset.application_category`` describes the type of software application.

:Cardinality: 0..\*
:Datatype: String
:Value constraint: Free value; no controlled vocabulary is imposed.
:Source: schema.org ``schema:applicationCategory``, also used by CodeMeta.
:Status: New field

Multiple categories may be provided when appropriate.

.. code-block:: json

   {
     "type": "software",
     "title": "FairFlow",
     "application_category": [
       "Workflow management"
     ]
   }

The value ``"Workflow management"`` is an example of a free-text category. The extension does not define a closed list of permitted categories.



Code repository
~~~~~~~~~~~~~~~

``dataset.code_repository`` identifies the source-code repository associated with the software.

:Cardinality: 0..1
:Datatype: URL
:Value constraint: None.
:Source: schema.org ``schema:codeRepository``, also used by CodeMeta.
:Status: New field

.. code-block:: json

   {
     "type": "software",
     "title": "FairFlow",
     "code_repository": "https://github.com/example-org/fairflow"
   }

The extension does not constrain the repository to a particular hosting platform.


Contributor
~~~~~~~~~~~

``dataset.contributor`` identifies persons or organisations that have contributed to the development of the software.

:Cardinality: 0..\*
:Datatype: Nested Data Structure
:Value constraint: None.
:Source: DCS, using the same structure as ``dmp.contributor``.
:Status: New field

A contributor may be either a person or an organisation. The contributor structure can contain identifying information and one or more roles describing the contribution.

.. code-block:: json

   {
     "contributor": [
       {
         "name": "John Smith",
         "contributor_id": {
           "identifier": "https://orcid.org/0000-0000-0000-0001",
           "type": "orcid"
         },
         "role": [
           "Coding",
           "Testing"
         ]
       }
     ]
   }

The proposed structure follows the existing DCS contributor structure.




Contributor role
~~~~~~~~~~~~~~~~

``dataset.contributor.role`` specifies the role played by a person or organisation in the development of the software.

:Cardinality: 0..\* per contributor
:Datatype: String
:Value constraint: Suggested values; the vocabulary is not closed.
:Source: DCS
:Status: Existing field, new vocabulary

The following values are suggested:

- Design
- Architecture
- Debugging
- Maintenance
- Coding
- Documentation
- Testing
- Support
- Management

Multiple roles may be associated with the same contributor. The suggested vocabulary is intentionally extensible: additional values may be used when none of the suggested roles adequately describes a contribution.

.. code-block:: json

   {
     "contributor": [
       {
         "name": "John Smith",
         "role": [
           "Coding",
           "Testing"
         ]
       },
       {
         "name": "Anna Nowak",
         "role": [
           "Documentation"
         ]
       }
     ]
   }




License
~~~~~~~

``dataset.license`` identifies the license or licenses under which the software is distributed.

:Cardinality: 0..\*
:Datatype: URL
:Value constraint: None.
:Source: DCAT (``dcterms:license``)
:Status: New field

Each value is represented as a URL identifying the applicable software distribution license. Multiple licenses may be specified when appropriate.

.. code-block:: json

   {
     "license": [
       "https://spdx.org/licenses/MIT"
     ]
   }

The example uses the SPDX URI for the MIT License.


Software distributions
----------------------

Distribution is optional. In the most basic setup, a software Dataset can be described without any Distribution. This is particularly useful at the beginning of a project, when the source code repository can be indicated through ``dataset.code_repository`` but no specific release or downloadable artifact has yet been produced.

Because DCS already supports the relationship between a Dataset and its Distributions, the proposed software extension does not prohibit the use of Distributions. They provide additional information when a concrete release or materialization of the software is available.

This follows the general DCAT logic: Dataset-level properties describe the software in general and are mostly independent of a particular implementation or release, whereas Distribution-level properties describe specific files or releases. The same release can therefore be represented by separate Distributions when it is made available through different services, such as GitHub and Zenodo.
