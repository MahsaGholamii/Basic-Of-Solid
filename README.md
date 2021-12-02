# Basic-Of-Solid
**#First one: Single Responsibility Principle (SRP)**


Basics Of SOLID Design Principles Using C# and .NET
S: Single Responsibility Principle (SRP)
O: Open closed Principle (OCP)
L: Liskov substitution Principle (LSP)
I: Interface Segregation Principle (ISP)
D: Dependency Inversion Principle (DIP)



**S: Single Responsibility Principle (SRP)**

Example:
public class UserService  
{  
   public void Register(string email, string password)  
   {  
      if (!ValidateEmail(email))  
         throw new ValidationException("Email is not an email");  
         var user = new User(email, password);  
  
         SendEmail(new MailMessage("mysite@nowhere.com", email) { Subject="HEllo foo" });  
   }
   public virtual bool ValidateEmail(string email)  
   {  
     return email.Contains("@");  
   }  
   public bool SendEmail(MailMessage message)  
   {  
     _smtpClient.Send(message);  
   }  
}   

**The SendEmail and ValidateEmail methods have nothing to do within the UserService class. Let's refract it.**


public class UserService  
{  
   EmailService _emailService;  
   DbContext _dbContext;  
   public UserService(EmailService aEmailService, DbContext aDbContext)  
   {  
      _emailService = aEmailService;  
      _dbContext = aDbContext;  
   }  
   public void Register(string email, string password)  
   {  
      if (!_emailService.ValidateEmail(email))  
         throw new ValidationException("Email is not an email");  
         var user = new User(email, password);  
         _dbContext.Save(user);  
         emailService.SendEmail(new MailMessage("myname@mydomain.com", email) {Subject="Hi. How are you!"});  
  
      }  
   }  
   public class EmailService  
   {  
      SmtpClient _smtpClient;  
   public EmailService(SmtpClient aSmtpClient)  
   {  
      _smtpClient = aSmtpClient;  
   }  
   public bool virtual ValidateEmail(string email)  
   {  
      return email.Contains("@");  
   }  
   public bool SendEmail(MailMessage message)  
   {  
      _smtpClient.Send(message);  
   }  
}   
