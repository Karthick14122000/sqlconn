create table regforms
(
id int identity(1,1) primary key,
name varchar(20),
email varchar(20),
pass varchar(20),
confpass varchar(20),
phone varchar(10),
gen varchar(10),
address varchar(100)
)
select *from regforms
insert into regforms values('karthi','karthick@gmail.com','karthick@1','karthick@1',6379668248,'male','palayam')

create procedure sp_reg
(
@Name varchar(20),
@Email varchar(20),
@Pass varchar(20),
@Confpass varchar(20),
@Phone varchar(20),
@Gen varchar(10),
@Address varchar(100)
)
as 
begin 
insert into regforms(name,email,pass,confpass,phone,gen,address) values(@Name,@Email,@Pass,@Confpass,@Phone,@Gen,@Address)
end
select *from regforms
================================================================================================================================================
            SqlConnection con = new SqlConnection("Data Source=DESKTOP-2JFC7AR;Integrated Security=true;Initial Catalog=registerpage");
            con.Open();
            SqlCommand com = new SqlCommand();
            com.Connection = con;
            com.CommandType = CommandType.StoredProcedure;
            com.CommandText = "sp_reg";
            com.Parameters.AddWithValue("@Name", txt_name.Text.ToString());
            com.Parameters.AddWithValue("@Phone", txt_num.Text.ToString());
            com.Parameters.AddWithValue("@Email", txt_email.Text.ToString());
            com.Parameters.AddWithValue("@Address", txt_address.Text.ToString());
            com.Parameters.AddWithValue("@Pass", txt_password.Text.ToString());
            com.Parameters.AddWithValue("@Confpass", txt_confpassword.Text.ToString());
            com.Parameters.AddWithValue("@Gen", txt_dropdownlist.Text.ToString());
            int k = com.ExecuteNonQuery();
            if (k != 0)
            {
                lbl_msg.Visible = true;
                lbl_msg.Text = "Records are Submit successfully";
            }
            con.Close();
=======================================================================================================================================================
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Data;
using System.Data.SqlClient;
using System.Configuration;

namespace jobproject
{
    public partial class homepage : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {

        }
=====================================grid view code
        protected void btn_show_Click(object sender, EventArgs e)
        {
            Get();
        }
        protected void Get()
        {
            SqlConnection Conn = new SqlConnection(ConfigurationManager.ConnectionStrings["karthickproj"].ConnectionString);
            SqlCommand command = new SqlCommand();
            SqlDataAdapter adapter = new SqlDataAdapter();
            DataSet ds = new DataSet();

            Conn.Open();
            command.Connection = Conn;
            command.CommandType = CommandType.StoredProcedure;
            command.CommandText = "sp_showreg";
            adapter = new SqlDataAdapter(command);
            adapter.Fill(ds);
            Conn.Close();
            GridView.DataSource = ds.Tables[0];
            GridView.DataBind();
        }
===sql code
create procedure sp_showreg
as
begin
select*from regis
end
================================================
        protected void Btn_submit_Click(object sender, EventArgs e)
        {
            SqlConnection conn = new SqlConnection(ConfigurationManager.ConnectionStrings["karthickproj"].ConnectionString);
            SqlCommand com = new SqlCommand();
            conn.Open();
            com.Connection = conn;
            com.CommandType = CommandType.StoredProcedure;
            com.CommandText = "sp_regis";
            com.Parameters.AddWithValue("@Names", txt_name.Text.ToString());
            com.Parameters.AddWithValue("@Gender", check_box.Text.ToString());
            com.Parameters.AddWithValue("@Email", txt_email.Text.ToString());
            com.Parameters.AddWithValue("@Address", txt_address.Text.ToString());
            com.Parameters.AddWithValue("@Exprience", txt_exp.Text.ToString());
            com.Parameters.AddWithValue("@Interset", txt_dropdownlist.Text.ToString());
            int k = com.ExecuteNonQuery();
            if (k != 0)
            {
                lbl_msg.Visible = true;
                lbl_msg.Text = "Records are Submit successfully";
            }
            conn.Close();
                              
        }

        protected void Btn_clear_Click(object sender, EventArgs e)
        { 
                txt_name.Text = "";
                txt_exp.Text = "";
                txt_email.Text = "";
                txt_dropdownlist.Text = "";
                txt_address.Text = "";
                check_box.Text = "";
        }
===================update
        protected void btn_update_Click(object sender, EventArgs e)
        {
            SqlConnection conn = new SqlConnection(ConfigurationManager.ConnectionStrings["karthickproj"].ConnectionString);
            SqlCommand com = new SqlCommand();
            conn.Open();
            com.Connection = conn;
            com.CommandType = CommandType.StoredProcedure;
            com.CommandText = "sp_updateregis";
            com.Parameters.AddWithValue("@Names", txt_name.Text.ToString());
            com.Parameters.AddWithValue("@Gender", check_box.Text.ToString());
            com.Parameters.AddWithValue("@Email", txt_email.Text.ToString());
            com.Parameters.AddWithValue("@Address", txt_address.Text.ToString());
            com.Parameters.AddWithValue("@Exprience", txt_exp.Text.ToString());
            com.Parameters.AddWithValue("@Interset", txt_dropdownlist.Text.ToString());
           
            conn.Close();
        }
=========sql for update
create procedure sp_updateregis
(
@Names varchar(20),
@Gender varchar(20),
@Email varchar(20),
@Address varchar(50),
@Exprience int,
@Interset varchar(30)
)
as begin 
update regis set Names=@Names, Gender=@Gender, Email=@Email, Address=@Address, Exprience=@Exprience, Interset=@Interset where Names=@Names
end

========================dlt
        protected void btn_del_Click(object sender, EventArgs e)
        {

            SqlConnection conn = new SqlConnection(ConfigurationManager.ConnectionStrings["karthickproj"].ConnectionString);
            SqlCommand com = new SqlCommand();
            conn.Open();
            com.Connection = conn;
            com.CommandType = CommandType.StoredProcedure;
            com.CommandText = "sp_deletereg";
            com.Parameters.AddWithValue("@Names", txt_name.Text.ToString());
   
            int k = com.ExecuteNonQuery();
            if (k != 0)
            {
                lbl_msg.Visible = true;
                lbl_msg.Text = "Records are Deleted successfully";
            }
            conn.Close();
        }
    }
}
============sql for dlt
create procedure sp_deletereg(@Names varchar(20))
as begin 
delete from regis where Names=@Names
end
===================================
login

         try
            {
                SqlConnection con = new SqlConnection("Data Source=DESKTOP-2JFC7AR;Integrated Security=true;Initial Catalog=registerpage");
                SqlDataAdapter sda = new SqlDataAdapter("SELECT COUNT(*) FROM  regforms WHERE email='" + txt_email.Text + "' AND pass='" + txt_pass.Text + "'", con);
                DataTable dt = new DataTable();
                sda.Fill(dt);
                if (dt.Rows[0][0].ToString() == "1")
                {
                    Response.Redirect("regs.aspx");

                }
            }
            catch
            {
                {
                    lbl.Text = "Login Unsuccessfull";
                }
            }
        }
         


========================================================================
------------------regpage------with radio-----------------
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace WebApplication1
{
    public partial class regs : System.Web.UI.Page
    {
        string gender;
        protected void Page_Load(object sender, EventArgs e)
        {

        }

        protected void Btn_submit_Click(object sender, EventArgs e)
        {
            SqlConnection con = new SqlConnection("Data Source=DESKTOP-2JFC7AR;Integrated Security=true;Initial Catalog=registerpage");
            con.Open();
            SqlCommand com = new SqlCommand();
            com.Connection = con;
            com.CommandType = CommandType.StoredProcedure;
            com.CommandText = "sp_reg";
            com.Parameters.AddWithValue("@Name", txt_name.Text.ToString());
            com.Parameters.AddWithValue("@Phone", txt_num.Text.ToString());
            com.Parameters.AddWithValue("@Email", txt_email.Text.ToString());
            com.Parameters.AddWithValue("@Address", txt_address.Text.ToString());
            com.Parameters.AddWithValue("@Pass", txt_password.Text.ToString());
            com.Parameters.AddWithValue("@Confpass", txt_confpassword.Text.ToString());
            com.Parameters.AddWithValue("@Gen", gender.ToString());
            int k = com.ExecuteNonQuery();
            if (k != 0)
            {
                lbl_msg.Visible = true;
                lbl_msg.Text = "Records are Submit successfully";
            }
            con.Close();
        }

        protected void radi1_CheckedChanged(object sender, EventArgs e)
        {
            gender = "male";

        }

        protected void radi2_CheckedChanged(object sender, EventArgs e)
        {
            gender = "female";
        }

        protected void radi3_CheckedChanged(object sender, EventArgs e)
        {
            gender = "others";

        }
    }
}
