# seedmail
简单的发送邮件
package com.limin.test.portal.basic.utils.mail;

import java.util.Properties;

import java.util.Date;    
import javax.mail.Address;    
import javax.mail.BodyPart;    
import javax.mail.Message;    
import javax.mail.MessagingException;    
import javax.mail.Multipart;    
import javax.mail.Session;    
import javax.mail.Transport;    
import javax.mail.internet.InternetAddress;    
import javax.mail.internet.MimeBodyPart;    
import javax.mail.internet.MimeMessage;    
import javax.mail.internet.MimeMultipart;

import org.apache.log4j.Logger;
public class SimpleMailSender {
	public Logger logger = Logger.getLogger(this.getClass());
	/**   
	  * 以文本格式发送邮件   
	  * @param mailInfo 待发送的邮件的信息   
	  */    
	    public boolean sendTextMail(MailSenderInfo mailInfo) {    
	      // 判断是否需要身份认证    
	      MyAuthenticator authenticator = null;    
	      Properties pro = mailInfo.getProperties();   
	      if (mailInfo.isValidate()) {    
	      // 如果需要身份认证，则创建一个密码验证器    
	        authenticator = new MyAuthenticator(mailInfo.getUserName(), mailInfo.getPassword());    
	      }   
	      // 根据邮件会话属性和密码验证器构造一个发送邮件的session    
	      Session sendMailSession = Session.getDefaultInstance(pro,authenticator);    
	      try {    
	      // 根据session创建一个邮件消息    
	      Message mailMessage = new MimeMessage(sendMailSession);    
	      // 创建邮件发送者地址    
	      Address from = new InternetAddress(mailInfo.getFromAddress());    
	      // 设置邮件消息的发送者    
	      mailMessage.setFrom(from);    
	      // 创建邮件的接收者地址，并设置到邮件消息中    
	      Address to = new InternetAddress(mailInfo.getToAddress());    
	      mailMessage.setRecipient(Message.RecipientType.TO,to);    
	      // 设置邮件消息的主题    
	      mailMessage.setSubject(mailInfo.getSubject());    
	      // 设置邮件消息发送的时间    
	      mailMessage.setSentDate(new Date());    
	      // 设置邮件消息的主要内容    
	      String mailContent = mailInfo.getContent();    
	      mailMessage.setText(mailContent);    
	      // 发送邮件    
	      Transport.send(mailMessage);   
	      return true;    
	      } catch (MessagingException ex) { 
	    	  logger.error("用户名密码错误，请查看邮件用户名密码！");
//	          ex.printStackTrace();    
	      }    
	      return false;    
	    }    
	       
	    /**   
	      * 以HTML格式发送邮件   
	      * @param mailInfo 待发送的邮件信息   
	      */    
	    public static boolean sendHtmlMail(MailSenderInfo mailInfo){    
	      // 判断是否需要身份认证    
	      MyAuthenticator authenticator = null;   
	      Properties pro = mailInfo.getProperties();   
	      //如果需要身份认证，则创建一个密码验证器     
	      if (mailInfo.isValidate()) {    
	        authenticator = new MyAuthenticator(mailInfo.getUserName(), mailInfo.getPassword());   
	      }    
	      // 根据邮件会话属性和密码验证器构造一个发送邮件的session    
	      Session sendMailSession = Session.getDefaultInstance(pro,authenticator);    
	      try {    
	      // 根据session创建一个邮件消息    
	      Message mailMessage = new MimeMessage(sendMailSession);    
	      // 创建邮件发送者地址    
	      Address from = new InternetAddress(mailInfo.getFromAddress());    
	      // 设置邮件消息的发送者    
	      mailMessage.setFrom(from);    
	      // 创建邮件的接收者地址，并设置到邮件消息中    
	      Address to = new InternetAddress(mailInfo.getToAddress());    
	      // Message.RecipientType.TO属性表示接收者的类型为TO    
	      mailMessage.setRecipient(Message.RecipientType.TO,to);    
	      // 设置邮件消息的主题    
	      mailMessage.setSubject(mailInfo.getSubject());    
	      // 设置邮件消息发送的时间    
	      mailMessage.setSentDate(new Date());    
	      // MiniMultipart类是一个容器类，包含MimeBodyPart类型的对象    
	      Multipart mainPart = new MimeMultipart();    
	      // 创建一个包含HTML内容的MimeBodyPart    
	      BodyPart html = new MimeBodyPart();    
	      // 设置HTML内容    
	      html.setContent(mailInfo.getContent(), "text/html; charset=utf-8");    
	      mainPart.addBodyPart(html);    
	      // 将MiniMultipart对象设置为邮件内容    
	      mailMessage.setContent(mainPart);    
	      // 发送邮件    
	      Transport.send(mailMessage);    
	      System.out.println("邮件发送成功！");
	      return true;    
	      } catch (MessagingException ex) {    
	          ex.printStackTrace();    
	      }    
	      return false;    
	    }  
	    
	    public static void main(String[] args) {
			
			String content = "";
			Runtime runtime = Runtime.getRuntime();

			MailSenderInfo mailSenderInfo = new MailSenderInfo();
//			mailSenderInfo.setMailServerHost("smtp.163.com");
			mailSenderInfo.setMailServerHost("smtp.exmail.qq.com");
			mailSenderInfo.setMailServerPort("465");
//			mailSenderInfo.setMailSocketFactoryPort("465");
//			mailSenderInfo.setFromAddress("huanqiu_123456@163.com");
			mailSenderInfo.setFromAddress("limin.zheng@algoblu.com");
//			mailSenderInfo.setToAddress("1922289648@qq.com");
			//String[] address = new String[]{"huanqiu_123456@163.com"};
			mailSenderInfo.setToAddress("zlm488@163.com");
//			mailSenderInfo.setUserName("huanqiu_123456@163.com");
			mailSenderInfo.setUserName("limin.zheng@algoblu.com");
//			mailSenderInfo.setPassword("123456a");
			mailSenderInfo.setPassword("algoblu123");
			/*mailSenderInfo.setToAddress("apply@algoblu.com");
			mailSenderInfo.setUserName("apply@algoblu.com");
			mailSenderInfo.setPassword("algoblu123");*/
			mailSenderInfo.setValidate(true);
			mailSenderInfo.setSubject("测试邮件口"+runtime);
			mailSenderInfo.setContent(content+runtime);
			
			/*for (int i = 0; i < 10; i++) {
				System.out.println("第"+i);
				sendHtmlMail(mailSenderInfo);// 发送html格式
				try {
					TimeUnit.SECONDS.sleep(600);
				} catch (InterruptedException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}*/
			
			sendHtmlMail(mailSenderInfo);// 发送html格式
			
		}

}
