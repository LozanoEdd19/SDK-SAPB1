# Connecting to SAP Business One Database

```
public class MySapBusinessOne
{
    private static readonly NLog.Logger Logger = NLog.LogManager.GetCurrentClassLogger();
    private readonly SAPbobsCOM.Company? oCompany;

    public MySapBusinessOne()
    {
        this.oCompany = new SAPbobsCOM.Company();
    }


    public bool SAPConnectCompany()
    {
        if (this.oCompany != null)
        {
            if (this.oCompany.Connected == true) { this.oCompany.Disconnect(); }

            this.oCompany.Server = "My ServerName";
            this.oCompany.language = SAPbobsCOM.BoSuppLangs.ln_Spanish_La;
            this.oCompany.UseTrusted = false;

            this.oCompany.DbServerType = SAPbobsCOM.BoDataServerTypes.dst_MSSQL2019;

            this.oCompany.DbUserName = "My DbUserName";
            this.oCompany.DbPassword = "My DbPassword";
            this.oCompany.CompanyDB = "My Company";
            this.oCompany.UserName = "My UserName";
            this.oCompany.Password = "My Password";

            int lRetCode = this.oCompany.Connect();

            if (lRetCode != 0)
            {
                this.oCompany.Disconnect();
                this.oCompany.GetLastError(out int lErrCode, out string sErrMsg);
                Logger.Error(new Exception(), String.Format("Error | {0}", lRetCode + " | " + sErrMsg));
                return false;
            }
            else
            {
                return true;
            }
        }
        else
        {
            Logger.Error(ex, String.Format("Error connecting to SAP"));
            return false;
        }
    }

    public bool SAPDisConnectCompany()
    {
        if (this.oCompany != null)
        {
            this.oCompany.Disconnect();
            return true;
        }
        else
        {
            Logger.Error(ex, String.Format("Error connecting to SAP"));
            return false;
        }
    }

}

```