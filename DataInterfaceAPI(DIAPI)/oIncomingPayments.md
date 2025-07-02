# Payments Object 

### Description
Payments is a business object that represents payment methods in the Banking module. This object defines the incoming payments from customers and outgoing payments to vendors. Available payment methods are cash, credit cards, checks, or bank transfers.

Source tables: ORCT (incoming payments) and OVPM (outgoing payments).

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


    public SapResponseModel AddIncomingPayments(EntityPayment document)
    {
        SAPbobsCOM.Payments oDocto;
        oDocto = (SAPbobsCOM.Payments)oCompany.GetBusinessObject(SAPbobsCOM.BoObjectTypes.oIncomingPayments);

        oDocto.DocDate = DateTime.Now.Date;
        oDocto.CardCode = document.cardCode;
        oDocto.DocCurrency = "MXP";

        if (document.typePayment == "cash")
        {
            oDocto.Remarks = "Cash";
            oDocto.CashAccount = document.cashAccount;
            oDocto.CashSum = document.cashSum;
        }
        if (document.typePayment == "trsfr")
        {
            oDocto.Remarks = "Transfer";
            oDocto.TransferAccount = document.transferAccount;
            oDocto.TransferReference = document.transferReference;
            oDocto.TransferSum = document.transferSum;
        }
        if (document.typePayment == "check")
        {
            oDocto.Remarks = "Check";
            oDocto.CheckAccount = document.checkAccount;
            oDocto.Checks.DueDate = DateTime.Now.Date;

            oDocto.Checks.CheckAccount = document.checkAccount;
            oDocto.Checks.CheckSum = document.checkSum;
            oDocto.Checks.CheckNumber = 1;
            oDocto.Checks.CountryCode = "MX";
            oDocto.Checks.Trnsfrable = SAPbobsCOM.BoYesNoEnum.tNO;
            oDocto.Checks.ManualCheck = SAPbobsCOM.BoYesNoEnum.tNO;
        }

        oDocto.Invoices.SetCurrentLine(0);
        oDocto.Invoices.DocEntry = document.docEntryInvoice;
        oDocto.Invoices.SumApplied = document.SumApplied;

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