# BusinessPartners 

### Description
BusinessPartners is a business object that represents the Business Partners Master Data in the Business Partners module.

This object enables you to:

- Add a business partner record. 
- Retrieve a business partner record by its key. 
- Update a business partner record. 
- Remove a business partner record. 
- Save the object in XML format. 

Source table: OCRD.


### Code

```
public class MySapBusinessOne
{
    private static readonly NLog.Logger Logger = NLog.LogManager.GetCurrentClassLogger();
    private readonly SAPbobsCOM.Company? oCompany;

    public MySapBusinessOne()
    {
        this.oCompany = new SAPbobsCOM.Company();
    }


    public bool AddBusinessPartner(EntitieBusinessPartner businessPartner)
    {
        SAPbobsCOM.BusinessPartners oDocto;
        oDocto = (SAPbobsCOM.BusinessPartners)oCompany.GetBusinessObject(SAPbobsCOM.BoObjectTypes.oBusinessPartners);

        oDocto.Series = 91; // optional

        oDocto.CardName = businessPartner.cardName;
        oDocto.CardType = SAPbobsCOM.BoCardTypes.cCustomer;
        oDocto.GroupCode = businessPartner.groupCode;
        oDocto.Currency = businessPartner.currency;
        oDocto.Phone1 = businessPartner.phone1;
        oDocto.Phone2 = businessPartner.phone2;
        oDocto.Cellular = businessPartner.cellular;
        oDocto.EmailAddress = businessPartner.eMail;
        oDocto.FederalTaxID = businessPartner.licTradNum;
        oDocto.SalesPersonCode = businessPartner.slpCode;
        oDocto.PriceListNum = businessPartner.listNum;
        oDocto.PayTermsGrpCode = businessPartner.groupNum;
        oDocto.AliasName = businessPartner.aliasName;
        oDocto.Industry = businessPartner.industryC;
        oDocto.Valid = SAPbobsCOM.BoYesNoEnum.tYES;
        oDocto.FreeText = businessPartner.freeText;


        oDocto.Addresses.SetCurrentLine(0);
        oDocto.Addresses.AddressType = SAPbobsCOM.BoAddressType.bo_BillTo;
        oDocto.Addresses.AddressName = "FISCAL";
        oDocto.Addresses.Street = businessPartner.street;
        oDocto.Addresses.Block = businessPartner.block;
        oDocto.Addresses.ZipCode = businessPartner.zipCode;
        oDocto.Addresses.City = businessPartner.city;
        oDocto.Addresses.Country = businessPartner.country;
        oDocto.Addresses.State = businessPartner.state;
        oDocto.Addresses.Add();

        oDocto.Addresses.SetCurrentLine(1);
        oDocto.Addresses.AddressType = SAPbobsCOM.BoAddressType.bo_ShipTo;
        oDocto.Addresses.AddressName = "ENTREGA";
        oDocto.Addresses.Street = businessPartner.street;
        oDocto.Addresses.Block = businessPartner.block;
        oDocto.Addresses.ZipCode = businessPartner.zipCode;
        oDocto.Addresses.City = businessPartner.city;
        oDocto.Addresses.Country = businessPartner.country;
        oDocto.Addresses.State = businessPartner.state;
        oDocto.Addresses.TaxCode = businessPartner.taxCode;
        oDocto.Addresses.Add();

        int lRetCode = oDocto.Add();
        if (lRetCode != 0)
        {
            int lErrCode;
            string sErrMsg;
            oCompany.GetLastError(out lErrCode, out sErrMsg);
            Logger.Error(new Exception(), String.Format("Error | {0}", lRetCode + " | " + sErrMsg));
            return false;
        }
        else
        {
            return true;
        }
    }

}

```