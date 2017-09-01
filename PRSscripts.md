use master
drop database if exists PRS
create database PRS
use PRS 

Create table [User] (
	Id int not null primary key identity (1,1),
	UserName varchar(20) not null,
	Password varchar(10) not null,
	FirstName varchar(20) not null,
	LastName varchar(20) not null,
	Phone varchar(12) not null,
	Email varchar(75) not null,
	IsReviewer bit not null default 0, --the user is not necessarily a reviewer so default is 0,-- 
	--but if they are then we would add a value in our insert--
	IsAdmin bit not null default 0, --same as above--
	Active bit not null default 1,
	DateCreated datetime not null default getdate(), 
	DateUpdated datetime,
	UpdatedByUser int foreign key references [User](id)
)

-- to support unique UserNames
create unique index IX_Username
on [User](Username)

--
Insert into [User] 
	(Username, Password, FirstName, LastName, Phone, Email, IsReviewer, IsAdmin) values
	('gpdoud', 'password', 'Greg', 'Doud', '513-703-7315', 'gdoud@maxtrain.com', 1, 1)


Create table Vendor (
	Id int not null primary key identity (1,1),
	Code varchar(10) not null,
	Name varchar(255) not null,
	Address varchar (255) not null,
	City varchar (255) not null,
	State varchar (2) not null,
	Zip varchar (5) not null,
	Phone varchar (12) not null,
	Email varchar (100) not null,
	IsPreAppoved bit not null default 0,
	Active bit not null default 1,
	DateCreated datetime not null default getdate(), 
	DateUpdated datetime,
	UpdatedByUser int foreign key references [User](id)
	)
go

create unique index IUX_Code
on Vendor(Code)

go

Insert into Vendor 
	(Code, Name, Address, City, State, Zip, Phone, Email, IsPreAppoved) values
	('SAM', 'Sam''s Club', '1234 Main Street', 'Cincinnati', 'OH', '45202',
	'513-777-0022', 'info@samsclub.com', 1)

Insert into Vendor 
	(Code, Name, Address, City, State, Zip, Phone, Email, IsPreAppoved) values
	('WAL', 'WalMart', '202 South Street', 'Cincinnati', 'OH', '45202',
	'513-222-5588', 'main@walmart.com', 1)

Insert into Vendor 
	(Code, Name, Address, City, State, Zip, Phone, Email, IsPreAppoved) values
	('AMZ', 'Amazon', '111 Third Street', 'Cincinnati', 'OH', '45202',
	'513-581-8968', 'info@amazon.com', 1)
go

Create table Product (
	Id int not null primary key identity (1,1),
	VendorID int foreign key references Vendor(id),
	PartNumber varchar(50) not null,
	Name varchar(150) not null,
	Price Decimal(10,2) not null,
	Unit varchar(255) not null,
	PhotoPath varchar(255),
	Active bit not null default 1,
	DateCreated datetime not null default getdate(), 
	DateUpdated datetime,
	UpdatedByUser int foreign key references [User](id)
	)
go

Insert into Product 
	(PartNumber, Name, Price, Unit) values
	('OFCDSK92', 'Office Desk', '899.00', '1')

Insert into Product 
	(PartNumber, Name, Price, Unit) values
	('LMP34', 'Lamp', '99.00', '2')

Insert into Product 
	(PartNumber, Name, Price, Unit) values
	('OFCCHR45', 'Office Chair', '299.00', '1')

Insert into Product 
	(PartNumber, Name, Price, Unit) values
	('DSKORG77', 'Desk Organizer', '39.98', '1')

Insert into Product 
	(PartNumber, Name, Price, Unit) values
	('FLCBT670', 'File Cabinet', '209.00', '1')

Insert into Product 
	(PartNumber, Name, Price, Unit) values
	('OFCCDNZA112', 'Office Credenza', '999.00', '1')

go

Create table Status (
	Id int not null primary key identity (1,1),
	Description varchar(20) not null,
	Active bit not null default 1,
	DateCreated datetime not null default getdate(), 
	DateUpdated datetime,
	UpdatedByUser int foreign key references [User](id)
	)

go

Insert into Status 
	(Description) values
	('New')

Insert into Status 
	(Description) values
	('Review')

Insert into Status 
	(Description) values
	('Approved')

Insert into Status 
	(Description) values
	('Rejected')

Insert into Status 
	(Description) values
	('Revise')

go

Create table PurchaseRequest (
	Id int not null primary key identity (1,1),
	UserId int foreign key references [User](id),
	Description varchar(100) not null,
	Justification varchar(255) not null,
	DateNeeded Datetime default dateadd(day,7,getdate ()),
	DeliveryMode varchar(25) not null,
	StatusId int foreign key references Status(id) default 1,
	Total Decimal(10,2) not null default 0,
	SubmittedDate datetime not null default getdate(),
	Active bit not null default 1,
	ReasonForRejection varchar(100) default '',
	DateCreated datetime not null default getdate(), 
	DateUpdated datetime,
	UpdatedByUser int foreign key references [User](id)
	)
go

Insert into PurchaseRequest
(Description, Justification, DeliveryMode) values
('Mahogany Oak Office Desk','New Hire', 'UPS')

go

Create table PurchaseRequestLineItem (
	Id int not null primary key identity (1,1),
	PurchaseRequestId int not null foreign key references PurchaseRequest(id),
	ProductId int not null foreign key references Product(id),
	Quantity int not null default 1
	)

go



