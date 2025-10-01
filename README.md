# data_migration_from_SQLserver_to_AzureBlobStorage

<img width="897" height="346" alt="image" src="https://github.com/user-attachments/assets/621fe8aa-14eb-4767-9cc7-6bb2806b1803" />

# step 1: install sql server management studion

# step 2: create a new login along with some configurations needed
- right click to login to create a new login credentials
  <img width="1114" height="986" alt="image" src="https://github.com/user-attachments/assets/e715f1e3-2afd-4472-841a-e0631c700c41" />

- choose SQL authetication
  <img width="942" height="807" alt="image" src="https://github.com/user-attachments/assets/a1827df7-ba6a-44b0-9167-b53cfc978af3" />

- select all the admin's rights
  <img width="934" height="801" alt="image" src="https://github.com/user-attachments/assets/1e0c47d3-e501-4d94-b7a1-409cb6fe900c" />

- Because I want to migrate north-wind database so I will select it. then click ok to create login
  <img width="935" height="820" alt="image" src="https://github.com/user-attachments/assets/cd8b6404-71f3-412b-8827-75fbc5b6b6e4" />

- now go to connect -> database engine
  <img width="431" height="878" alt="image" src="https://github.com/user-attachments/assets/63eb8c8d-8fee-4eaa-9346-a22704892cdf" />

- choose sql server authetication to login
  <img width="601" height="743" alt="image" src="https://github.com/user-attachments/assets/a2068fd7-532e-41fa-ac85-5d6369eb20fc" />

# step 3: create a blob storage
<img width="1856" height="816" alt="image" src="https://github.com/user-attachments/assets/ecb35a6e-5bfd-4d42-b9be-a5588f676548" />

- for redundancy -> choose locally-redundant storage, for other settings -> leave them default
  <img width="1225" height="807" alt="image" src="https://github.com/user-attachments/assets/f5bfff93-3fdb-4793-b3a3-7e34bea9888f" />

- once the storage account is created -> create a new container called 'stage'
  <img width="1886" height="839" alt="image" src="https://github.com/user-attachments/assets/15877865-0016-419f-a21e-92b82ed2552b" />

# step 4: create ADF and connect it to SSMS
<img width="839" height="816" alt="image" src="https://github.com/user-attachments/assets/a8d20381-b8e1-43d2-895f-37c74c63b887" />

- once created -> launch data factory -> create a new pipeline -> add 'copy' activity
  <img width="1881" height="825" alt="image" src="https://github.com/user-attachments/assets/75f5afe3-f6dc-4a9e-bcf3-4d5a1512ac04" />

- go to source -> create a new source dataset -> choose sql server
  <img width="1906" height="860" alt="image" src="https://github.com/user-attachments/assets/caa81cc6-76f6-46cf-9315-c81977c22e80" />

- create linked service for it, remember to create a new intergration runtime because the default one (autoResolveIntergrationRuntine) only works with data movement inside Azure.
- therfor, I need to create a self-hosted one
  <img width="824" height="804" alt="image" src="https://github.com/user-attachments/assets/79866d47-0da6-4089-92d7-3c549c413f2f" />

- onece the intergration runtime is created I should see the screen like this
    <img width="835" height="843" alt="image" src="https://github.com/user-attachments/assets/a751e9ec-0d78-439e-80c1-a14f79c23592" />

- choose option 1 express setup
- once it done installed you will see the new integration runtime
  <img width="790" height="844" alt="image" src="https://github.com/user-attachments/assets/5c14320d-a647-475d-b6bf-fa4514865692" />
- then put other informations: servername, username, password, database (pick north-wind because I need to migrate it)
  <img width="722" height="833" alt="image" src="https://github.com/user-attachments/assets/1892c76f-2285-41ed-9aeb-0e48a775e3f3" />

- then click test connection
  <img width="818" height="840" alt="image" src="https://github.com/user-attachments/assets/a0865a2b-e856-4259-8c02-a399dc9bde84" />

- if test connection is failed then go to Microsoft integration runtime manager and change the settings to enable 'remote access from intranet'
- then choose which table inside the noth_wind database that I want to migrate
- click preview data, here I can see the data inside the Order table
  <img width="1571" height="892" alt="image" src="https://github.com/user-attachments/assets/5c0ace75-5a89-44ff-b076-9dfcffbfa42c" />

- next create a sink dataset -> blob storage -> csv format
- this time choose AutoResolveIntegrationRunTime because data is now inside Azure. and don't forget to test connection
  <img width="812" height="813" alt="image" src="https://github.com/user-attachments/assets/d766d058-a79c-4b9b-b1da-cfbfc5c3bd46" />

- in the file path -> choose 'stage' container that I created before
  <img width="795" height="825" alt="image" src="https://github.com/user-attachments/assets/111656cb-40d5-4aea-af86-a7900cc0a9f0" />

  
