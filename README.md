CREATING NEW WEB PROJECT SYMFONY
symfony new gestion_salle --webapp

INSTALLING TAILWIND
composer require symfonycasts/tailwind-bundle
php bin/console tailwind:init
php bin/console tailwind:build --watch

ENTITIES
EVENTTYPE
symfony console make:entity EventType
name varchar 255 not null
description varchar 255 nullable

    ADDRESS
    	symfony console make:entity Address
    	number int not null
    	street varchar 255 not null
    	country varchar120 not null
    	city varchar120 not null
    	codePostal varchar120 not null

    ERGONOMY
    	symfony console make:entity Ergonomy
    	name varchar180 not null
    	description varchar255  nullable

    EQUIPMENT
    	symfony console make:entity Equipment
    	name varchar180 not null
    	description varchar255  nullable
    	type varchar180 not null

    USER
    	symfony console make:user

    NOTIFICATION
    	symfony console make:entity Notification
    	message varchar255 not null
    	createdAt datetime_immutable not null
    	isRead boolean not null
    	userId M21 with User (Notification.userId allowed nullable?: No, OrphanRemoval?: no)


    HALL
    	symfony console make:entity Hall
    	name varchar255 not null
    	area varchar120 not null
    	accessibility varchar255 not null
    	capacityMax integer not null
    	pricePerHour decimal not null (Precision 5, scale 2)
    	openingTime time not null
    	closingTime time not null
      	eventTypeId 121 with EventType(Hall.eventTypeId nullable?: No, eventType->getHall(): yes)
      	addresseId 121 with Address(Hall.addressId nullable?: No)
    	mainImg varchar255 not null


    RESERVATION
    	symfony console make:entity Reservation
    	startDate date not null
    	endDate date not null
    	startTime time not null
    	endTime time not null
    	isConfirmed boolean not null
    	specialRequest varchar 255 nullable
    	userId ManyToOne with User (Reservation.userId nullable?: No, OrphanRemoval? :no)
    	hallId ManyToOne with Hall( Reservation.hallId nullable? : No, OrphanRemoval? : no)
    	createdAt datetime_immutable  not null
    	updatedAt datetime_immutable  not null

.env.local
database name db_hall_management

CREATION BDD
symfony console d:d:c
symfony console make:migration
symfony console d:m:m

FIXTURES
composer require orm-fixtures --dev
composer require fakerphp/faker

LOADED THE FIXTURES
symfony console d:f:l

app_hall_edit
app_hall_show
app_hall_show_all
app_hall_delete

app_ergonomy_edit
app_ergonomy_delete

app_equipment_edit

CREATING SECURITY CONTROLLER AND REGISTRATION FORM
symfony console make:security:form-login
SecurityController with /logout and no phpUnit test

INSTALLED UX ICON FROM SYMFONY
composer require symfony/ux-twig-component
composer require symfony/ux-icon

ADMIN
composer req easycorp/easyadmin-bundle
symfony console make:admin:dashboard
symfony console make:admin:crud

CREATING HOME CONTROLLER

## symfony console make:controller HomeController

CREATING PROFILE CONTROLLER

## symfony console make:controller ProfileController

CREATING HALL CONTROLLER

## symfony console make:controller HallController

CREATING NOTIFICATION CONTROLLER

## symfony console make:controller NotificationController

CREATING RESERVATION CONTROLLER

## symfony console make:controller ReservationController

    symfony console make:registration-form (UniqueEntity validation?: Yes, send Email?: Yes, include userid in verifcation link?: No, email?: hall4all@email.com,name?:  hall4all)
    	 composer require symfonycasts/verify-email-bundle

    To launch the server mailer
    	symfony console messenger:consume async -vv

ADDED NEW ENTITY
symfony console make:entity Images
title varchar 120 not null
img varchar255 not null

symfony console make:entity HallEquipment
hallId M21 with Hall (HallEquipment.hallId nullable?: No, $hall->getHallEquipment()?: yes , orphanRemoval?: Yes)
equipmentId M21 with equipment

symfony console make:entity HallErgonomy
hallId M21 with Hall
ergonomyId M21 with Ergonomy

symfony console make:entity HallImage
hallId M21 with Hall
imgId M21 with Images

ERROR
wrong relation with intermidiate tables from the intermidiate point its M21 with Hall and M21 with Ergonomy
121 relation with EventType and Addresse which was blocking also the same problem of duplicate key entry.
