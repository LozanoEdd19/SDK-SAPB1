# Items

### Description
Items is a business object that represents the items master data in the Inventory and Production module.

This object enables you to:

- Add an item details. 
- Retrieve an item by its key. 
- Update item details. 
- Save the object in XML format. 

Source table: OITM.


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


    public bool AddItems(EntitieItems items)
    {
        SAPbobsCOM.Items oDocto;
        oDocto = (SAPbobsCOM.Items)oCompany.GetBusinessObject(SAPbobsCOM.BoObjectTypes.oItems);

        oDocto.ItemCode = item.itemCode;
        oDocto.ItemName = item.itemName;
        oDocto.ForeignName = item.frgnName;
        oDocto.ItemsGroupCode = item.itmsGrpCod;

        oDocto.PurchaseItem = SAPbobsCOM.BoYesNoEnum.tNO;
        oDocto.SalesItem = SAPbobsCOM.BoYesNoEnum.tYES;
        oDocto.InventoryItem = SAPbobsCOM.BoYesNoEnum.tYES;

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
