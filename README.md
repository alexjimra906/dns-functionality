# DNS Configuration and Management

## **Objective**
This lab demonstrates the configuration and testing of DNS (Domain Name System) services, including A-record creation, local DNS cache management, and CNAME record testing, to ensure proper name resolution within an Active Directory environment.

---

## **Environments and Technologies Used**
- **Microsoft Azure**: Hosting virtual machines for the lab environment.
- **Active Directory Domain Services (AD DS)**: Used for managing DNS records.
- **Command Line Utilities**: `ping`, `nslookup`, `ipconfig`.
- **Windows Server 2022**: Domain Controller.
- **Windows 10**: Client machine for validation.

---

## **Steps and Details**

### **1. A-Record Exercise**
1. **Setup**
   - Log in to **DC-1** as `mydomain.com\jane_admin`.
   - Log in to **Client-1** as `mydomain\jane_admin`.

2. **Testing Name Resolution**
   - From Client-1, attempt to `ping mainframe`.  
     _Expected Outcome:_ The ping fails due to the absence of a DNS record.  
    ![image](https://github.com/user-attachments/assets/bcae2019-7afc-4b8f-a440-4a46294094ff)


   - Run `nslookup mainframe` from Client-1.  
     _Expected Outcome:_ The lookup fails as there’s no corresponding DNS record.  
     ![image](https://github.com/user-attachments/assets/a46f756f-4a2b-4203-b190-187ada40ebc9)


3. **Create a DNS A-Record**
   - On **DC-1**, create an A-record for `mainframe` pointing to the DC’s private IP address.
     ![image](https://github.com/user-attachments/assets/eb025f2a-4d74-433d-acee-33fa549c1e77)


4. **Validate**
   - From **Client-1**, run `ping mainframe`.  
     _Expected Outcome:_ The ping now succeeds.  
     ![image](https://github.com/user-attachments/assets/4a07b2b7-d73a-4e9c-bb08-29ceefcd05cf)

---

### **2. Local DNS Cache Exercise**
1. **Modify DNS Record**
   - On **DC-1**, change the A-record for `mainframe` to point to `8.8.8.8`.
     ![image](https://github.com/user-attachments/assets/8f51e3f4-f2cf-484e-ae0c-3f17d55da227)


2. **Testing**
   - From **Client-1**, run `ping mainframe`.  
     _Expected Outcome:_ The ping resolves to the old address due to cached DNS.  
     ![image](https://github.com/user-attachments/assets/e2164622-99d7-43be-a207-00bdf83b43a5)


3. **Flush DNS Cache**
   - On **Client-1**, run `ipconfig /flushdns`.
     ![image](https://github.com/user-attachments/assets/2137eca5-8cbf-4dc2-a1a6-417e816e6dd5)

   - Verify the DNS cache is cleared using `ipconfig /displaydns`.  
     _Expected Outcome:_ The cache is empty.  
     ![image](https://github.com/user-attachments/assets/3eddb34e-b83b-4e45-bb92-ef2c0d9ee182)



4. **Revalidate**
   - Run `ping mainframe` again.  
     _Expected Outcome:_ The ping now resolves to the updated address (`8.8.8.8`).  
     ![image](https://github.com/user-attachments/assets/e9be001d-f524-4df2-aebb-13c7416b7999)


---

### **3. CNAME Record Exercise**
1. **Setup**
   - On **DC-1**, create a CNAME record mapping `search` to `www.google.com`.
     ![image](https://github.com/user-attachments/assets/cb2824fb-8d69-4e9c-8889-762bca274f3b)


2. **Testing**
   - From **Client-1**, run `ping search`.  
     _Expected Outcome:_ The ping resolves to Google’s IP address.  
     ![image](https://github.com/user-attachments/assets/112624c1-c36a-4c5d-8e11-8f76c1f3a7c5)


   - Run `nslookup search` from Client-1.  
     _Expected Outcome:_ The lookup confirms the CNAME record.  
     ![image](https://github.com/user-attachments/assets/d0efa983-6fd4-4c31-b49e-8825d8f5df55)


---

## **Conclusion**
This lab highlights the practical steps required to manage and validate DNS configurations within an Active Directory environment. By completing these exercises, we learned how to:
- Create and modify DNS records.
- Manage and flush DNS caches.
- Test name resolution using CNAME and A-records.

---
