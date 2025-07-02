# Documents Object 
Documents(oOrders) - ORDR 

### Description
Documents is a business object that represents the header data of documents in the Marketing Documents and Receipts module and the Inventory and Production module of SAP Business One application.

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


    public SapResponseModel AddOrders(EntitiesDocument document)
    {
        SAPbobsCOM.Documents oDocto;
        oDocto = (SAPbobsCOM.Documents)oCompany.GetBusinessObject(SAPbobsCOM.BoObjectTypes.oOrders);

        oDocto.DocDate = DateTime.Now.Date;
        oDocto.DocDueDate = DateTime.Now.Date;
        oDocto.CardCode = document.cardCode;
        oDocto.Comments = document.comments;

        oDocto.DocType = SAPbobsCOM.BoDocumentTypes.dDocument_Items;
        oDocto.DocCurrency = document.docCurrency;
        oDocto.Comments = document.comments;
        oDocto.NumAtCard = document.numAtCard;
        oDocto.SalesPersonCode = document.slpCode;
        oDocto.ShipToCode = document.shipToCode;

        for (int i = 0; i < document.lines.Count; i++)
        {
            oDocto.Lines.SetCurrentLine(i);
            oDocto.Lines.ItemCode = lines[i].itemCode;
            oDocto.Lines.Quantity = lines[i].quantity;

            oDocto.Lines.Price = lines[i].price;
            oDocto.Lines.UnitPrice = lines[i].unitPrice;
            oDocto.Lines.DiscountPercent = lines[i].discountPercent;

            oDocto.Lines.TaxCode = lines[i].taxCode;
            oDocto.Lines.Currency = lines[i].currency;
            oDocto.Lines.WarehouseCode = lines[i].warehouseCode;
        }

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