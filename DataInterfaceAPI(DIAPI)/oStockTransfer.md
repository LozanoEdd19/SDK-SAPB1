# StockTransfer Object 

### Description
StockTransfer is a business object that represents items to transfer from one warehouse to another. This object is part of the Inventory and Production module. 

Source tables: OWTR (ODRF for drafts)

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


    public SapResponseModel AddStockTransfer(EntityStockTransfer document)
    {
        SAPbobsCOM.StockTransfer oDocto;
        oDocto = (SAPbobsCOM.StockTransfer)oCompany.GetBusinessObject(SAPbobsCOM.BoObjectTypes.oStockTransfer);

        oDocto.DocDate = DateTime.Now.Date;
        oDocto.TaxDate = document.taxDate;
        oDocto.CardCode = document.cardCode;
        oDocto.Comments = document.comments;
        oDocto.FromWarehouse = document.fromWarehouseCode;
        oDocto.ToWarehouse = document.warehouseCode;
        oDocto.PriceList = oDocto.priceList;
        oDocto.JournalMemo = oDocto.journalMemo;

        for (int i = 0; i < document.lines.Count; i++)
        {
            oDocto.Lines.SetCurrentLine(i);
            oDocto.Lines.ItemCode = lines[i].itemCode;
            oDocto.Lines.Quantity = lines[i].quantity;
            oDocto.Lines.Price = lines[i].price;
            oDocto.Lines.UnitPrice = lines[i].price;
            oDocto.Lines.DiscountPercent = lines[i].discountPercent;
            oDocto.Lines.Currency = lines[i].currency;
            oDocto.Lines.WarehouseCode = lines[i].warehouseCode;
            oDocto.Lines.FromWarehouseCode = lines[i].fromWarehouseCode;
            oDocto.Lines.Add();
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