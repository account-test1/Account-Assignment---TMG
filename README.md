# Account-Assignment---TMG
RAP Table Maintenance Generator
<img width="1583" height="810" alt="image" src="https://github.com/user-attachments/assets/b001d049-92ba-44dc-8bdd-9b635d526d22" />
**************************************Consumption view**********************************
@AccessControl.authorizationCheck: #CHECK
@Search.searchable: true
@Metadata.allowExtensions: true
@VDM.viewType: #CONSUMPTION
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define root view entity ZC_ACCT_ASSIGNT
  provider contract transactional_query
  as projection on ZI_ACCT_ASSIGNT
{
  key bukrs,
      @Search.defaultSearchElement: true
      @Search.ranking: #HIGH
  key OrderType,
  key StatusProfile,
  key SysStatus,
  key UserStatus,
      Include,
      Exclude,
      Active,
      Createdby,
      Createdon,
      Lastmodifiedby,
      Lastchangedat,
      Locallastchangedat
}
**************************************************************************************************
********************************Draft Table**************************************************
<img width="840" height="681" alt="image" src="https://github.com/user-attachments/assets/1d632200-b6e1-4af3-bc22-5cd06c42822a" />
***************************************Interface view for draft table***************************
@AccessControl.authorizationCheck: #CHECK
@EndUserText.label: 'Interface View for Draft table'
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
@VDM.viewType: #BASIC
define view entity ZI_ACCT_ASSIGNT_D as select from zfi_acc_3em_d
{
    key bukrs as Bukrs,
    key ordertype as Ordertype,
    key statusprofile as Statusprofile,
    key sysstatus as Sysstatus,
    key userstatus as Userstatus,
    include as Include,
    exclude as Exclude,
    active as Active,
    createdby as Createdby,
    createdon as Createdon,
    lastmodifiedby as Lastmodifiedby,
    lastchangedat as Lastchangedat,
    locallastchangedat as Locallastchangedat,
    draftentitycreationdatetime as Draftentitycreationdatetime,
    draftentitylastchangedatetime as Draftentitylastchangedatetime,
    draftadministrativedatauuid as Draftadministrativedatauuid,
    draftentityoperationcode as Draftentityoperationcode,
    hasactiveentity as Hasactiveentity,
    draftfieldchanges as Draftfieldchanges
}
*********************************************************************************************************
*******************************************VALUE HELP***************************************************
@EndUserText.label: 'VIM Type of Order ValueHelp'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@VDM.viewType: #BASIC
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define view entity Z_ACC_TYPE_ORDER_VH

  as select from dd07t
{
      @ObjectModel.text.element: ['Ffdesc']
  key domvalue_l as OrdType,
      @Semantics.text: true
      ddtext     as Ffdesc
}
where
      domname    = 'ZFI_ORDTYP'
  and ddlanguage = 'E'
  and as4local   = 'A'
  **************************************************************************************************************
  ************************ACCESS CONTROL FOR VALUE HELP*********************************************************
  @EndUserText.label: 'Access Control for Z_ACC_TYPE_ORDER_VH'
@MappingRole: true
define role Z_ACC_TYPE_ORDER_VH {
grant
        select
            on
            Z_ACC_TYPE_ORDER_VH
            where
              ( ) = aspect pfcg_auth(S_TABU_NAM, ACTVT = '03', TABLE = 'ZFI_ACC_ASSG_3EM' );
                        
}
***********************************ACCESS CONTROL FOR COMPANY CODE******************************************************
@EndUserText.label: 'Access control for ZI_ACCT_ASSIGNT_D'
@MappingRole: true
define role ZI_ACCT_ASSIGNT_D {
    grant
        select
            on
                ZI_ACCT_ASSIGNT_D
                    where
                       (bukrs) = aspect pfcg_auth(F_BKPF_BUK,BUKRS, ACTVT = '03');
                        
}
*************************************************************************************************************************
************************************************METADATA EXTENSION*******************************************************
@Metadata.layer: #CORE
@UI: {
    headerInfo: { typeName: 'Company Code',
                  typeNamePlural: 'Company Code',
                  title: { type: #STANDARD, label: 'Company Code', value: 'Bukrs' }
                          } }
annotate view ZC_ACCT_ASSIGNT with
{
  @UI.facet: [{ id: 'CompanyCode',
                 purpose: #STANDARD,
                 type: #IDENTIFICATION_REFERENCE,
                 label: 'Accnt. assign interface from S4 to 3EM',
                 position: 10 }]

  @UI: { lineItem: [{ position: 10 , label: 'CompanyCode' }],
         identification: [{ position: 10, label: 'CompanyCode' }]
         }
  @Consumption.valueHelpDefinition: [ { entity: { name: 'I_CompanyCodeStdVH', element: 'CompanyCode' } } ]
  bukrs;

  @UI: { lineItem: [{ position: 20 , label: 'Type Of Order' }],
         identification: [{ position: 20, label: 'Type Of Order' }]
         }
  @Consumption.valueHelpDefinition: [ { entity: { name: 'Z_ACC_TYPE_ORDER_VH', element: 'OrdType' } } ]
  OrderType;
  @UI: { lineItem: [{ position: 30 , label: 'StatusProfile' }],
         identification: [{ position: 30, label: 'StatusProfile' }]
         }

  StatusProfile;
  @UI: { lineItem: [{ position: 40 , label: 'SysStatus' }],
         identification: [{ position: 40, label: 'SysStatus' }]
         }
  SysStatus;
  @UI: { lineItem: [{ position: 50 , label: 'UserStatus' }],
         identification: [{ position: 50, label: 'UserStatus' }]
         }
  UserStatus;
  @UI: { lineItem: [{ position: 60 , label: 'Include' }],
         identification: [{ position: 60, label: 'Include' }]
         }
  Include;
  @UI: { lineItem: [{ position: 70 , label: 'Exclude' }],
         identification: [{ position: 70, label: 'Exclude' }]
         }
  Exclude;
  @UI: { lineItem: [{ position: 80 , label: 'Active' }],
         identification: [{ position: 80, label: 'Active' }]
         }
  Active;
  @UI.lineItem: [{ position: 90 }]
  Createdby;
  @UI.lineItem: [{ position: 100 }]
  Createdon;
  @UI.lineItem: [{ position: 110 }]
  Lastmodifiedby;
  @UI.lineItem: [{ position: 120 }]
  Lastchangedat;
  @UI.lineItem: [{ position: 130 }]
  Locallastchangedat;

}
**********************************************BEHAVIOR DEFINITION********************************************
managed implementation in class zbp_i_acct_assignt unique;
strict;
with draft;

define behavior for ZI_ACCT_ASSIGNT alias I_ACCT_ASSIGNT
persistent table zfi_acc_assg_3em
draft table zfi_acc_3em_d query ZI_ACCT_ASSIGNT_D
lock master total etag Lastchangedat
authorization master ( instance )
etag master Locallastchangedat
{
  field ( mandatory : create, readonly : update ) bukrs;
  field ( readonly ) CreatedBy, CreatedOn, lastmodifiedby, LastChangedAt;

  create ( precheck );
  update;
  delete;
  draft action Edit;
  draft action Activate;
  draft action Discard;
  draft action Resume;
  draft determine action Prepare;

  mapping for zfi_acc_assg_3em
  {
    bukrs = bukrs;
    OrderType = type;
    StatusProfile = status_profile;
    SysStatus = sys_status;
    UserStatus = user_status;
    Include = include;
    Exclude = exclude;
    Active = entry_active;
    Createdby = createdby;
    Createdon = createdon;
    Lastmodifiedby = lastmodifiedby;
    Locallastchangedat = locallastchangedat;

  }
}
***********************************************BEHAVIOR CONSUMPTION**************************************************
projection;
strict;
use draft;

define behavior for ZC_ACCT_ASSIGNT alias C_ACCT_ASSIGNT
{
  use create;
  use update;
  use delete;
  use action Edit;
  use action Activate;
  use action Discard;
  use action Resume;
  use action Prepare;
}
*****************************************BEHAVIOR POOL CLASS*************************************************************
*//*----------------------------------------------------------------------*
*//* Development ID:  ZDDE-00084797                                       *
*//* Functional Spec: ZFSE-00084796                                       *
*//* Description: Account assignment interface from S4 to 3EM             *
*//*              Behavior Implementation for ZI_ACCT_ASSIGNT             *
*//*----------------------------------------------------------------------*
*//*  Change Log:                                                         *
*//* ---------------------------------------------------------------------*
*//*    Who       Date           CR            Text                       *
*//*  CHINTRE3   12-APR-2024    1100244400    Initial Development         *
*//*----------------------------------------------------------------------*
CLASS lhc_i_acct_assignt DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR i_acct_assignt RESULT result.

    METHODS precheck_create FOR PRECHECK
      IMPORTING entities FOR CREATE i_acct_assignt.

    METHODS bukrs_auth
      IMPORTING
        iv_company_code         TYPE bukrs
        iv_activity             TYPE activ_auth
      RETURNING
        VALUE(rv_is_authorized) TYPE abap_bool.

ENDCLASS.

CLASS lhc_i_acct_assignt IMPLEMENTATION.

  METHOD get_instance_authorizations.

*- Reading the Entries of the entity
    READ ENTITIES OF zi_acct_assignt IN LOCAL MODE
    ENTITY i_acct_assignt
    ALL FIELDS WITH CORRESPONDING #( keys )
    RESULT DATA(lt_acct_assignt)
    FAILED DATA(lt_failed).

*- Checking if the internal table is not initial
    IF lt_acct_assignt[] IS NOT INITIAL.

*- Checking the authorization Object
      LOOP AT lt_acct_assignt ASSIGNING FIELD-SYMBOL(<fs_acct_assignt>).

        APPEND VALUE #( %is_draft        = <fs_acct_assignt>-%is_draft
                         %key            = <fs_acct_assignt>-%key
                         %update         = COND #( WHEN bukrs_auth( iv_company_code = <fs_acct_assignt>-bukrs
                                                                    iv_activity     = gc_activity-change ) EQ abap_true
                                                   THEN if_abap_behv=>auth-allowed
                                                   ELSE if_abap_behv=>auth-unauthorized )
                         %delete         = COND #( WHEN bukrs_auth( iv_company_code = <fs_acct_assignt>-bukrs
                                                                    iv_activity     = gc_activity-delete ) EQ abap_true
                                                   THEN if_abap_behv=>auth-allowed
                                                   ELSE if_abap_behv=>auth-unauthorized )
                         %action-edit    = COND #( WHEN bukrs_auth( iv_company_code = <fs_acct_assignt>-bukrs
                                                                    iv_activity     = gc_activity-change ) EQ abap_true
                                                   THEN if_abap_behv=>auth-allowed
                                                   ELSE if_abap_behv=>auth-unauthorized )
                       ) TO result.

      ENDLOOP.
    ENDIF.
  ENDMETHOD.

  METHOD precheck_create.
    LOOP AT entities ASSIGNING FIELD-SYMBOL(<ls_entity>).
      IF bukrs_auth( iv_company_code = <ls_entity>-%key-bukrs
                                    iv_activity     = gc_activity-create ) EQ abap_false.

        APPEND VALUE #( %key =  <ls_entity>-%key ) TO failed-i_acct_assignt.
        APPEND VALUE #( %key =  <ls_entity>-%key
                        %msg = new_message( id       = lc_msgid
                                            number   = lc_002
                                            severity = if_abap_behv_message=>severity-error )
                                           ) TO reported-i_acct_assignt.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.

  METHOD bukrs_auth.

    AUTHORITY-CHECK OBJECT lc_f_bkpf_buk
      ID lc_actvt FIELD iv_activity
      ID lc_bukrs FIELD iv_company_code.

    IF sy-subrc = 0.
      DATA(lv_is_authorized) = abap_true.
    ENDIF.

    rv_is_authorized = lv_is_authorized.
  ENDMETHOD.

ENDCLASS.
************************************************SERVICE DEFINITION********************************************
@EndUserText.label: 'Service Definition for ZC_ACCT_ASSIGNT'
define service ZC_SD_ACCT_ASSIGNT {
  expose ZC_ACCT_ASSIGNT as ACCT_ASSIGNT;
}
