# vanfest
This is a repository of all of my VanFest USA SQL documents to create and manage the database. It's an example of what I've done in my database design class at BYU-Idaho.

## This is my repository for Van Fest

##  Here is the database creation and insert statements

-- MySQL Workbench Forward Engineering Van Fest Database By Ashton Glines

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema vanfest
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema vanfest
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `vanfest` DEFAULT CHARACTER SET utf8 ;
USE `vanfest` ;

-- -----------------------------------------------------
-- Table `vanfest`.`zip`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `vanfest`.`zip` ;

CREATE TABLE IF NOT EXISTS `vanfest`.`zip` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `zip` CHAR(5) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `id_UNIQUE` (`id` ASC) VISIBLE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `vanfest`.`state`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `vanfest`.`state` ;

CREATE TABLE IF NOT EXISTS `vanfest`.`state` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `state` CHAR(2) NOT NULL,
  `zip_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `id_UNIQUE` (`id` ASC) VISIBLE,
  INDEX `fk_state_zip_idx` (`zip_id` ASC) VISIBLE,
  CONSTRAINT `fk_state_zip`
    FOREIGN KEY (`zip_id`)
    REFERENCES `vanfest`.`zip` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `vanfest`.`city`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `vanfest`.`city` ;

CREATE TABLE IF NOT EXISTS `vanfest`.`city` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `city` VARCHAR(45) NOT NULL,
  `state_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `id_UNIQUE` (`id` ASC) VISIBLE,
  INDEX `fk_city_state1_idx` (`state_id` ASC) VISIBLE,
  CONSTRAINT `fk_city_state1`
    FOREIGN KEY (`state_id`)
    REFERENCES `vanfest`.`state` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `vanfest`.`address`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `vanfest`.`address` ;

CREATE TABLE IF NOT EXISTS `vanfest`.`address` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `address_1` VARCHAR(45) NOT NULL,
  `address_2` VARCHAR(45) NULL,
  `city_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `id_UNIQUE` (`id` ASC) VISIBLE,
  INDEX `fk_address_city1_idx` (`city_id` ASC) VISIBLE,
  CONSTRAINT `fk_address_city1`
    FOREIGN KEY (`city_id`)
    REFERENCES `vanfest`.`city` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `vanfest`.`exhibitor`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `vanfest`.`exhibitor` ;

CREATE TABLE IF NOT EXISTS `vanfest`.`exhibitor` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `first_name` VARCHAR(45) NOT NULL,
  `last_name` VARCHAR(45) NOT NULL,
  `business_name` VARCHAR(45) NULL,
  `social_handle` VARCHAR(45) NULL,
  `email` VARCHAR(45) NOT NULL,
  `phone` CHAR(10) NOT NULL,
  `address_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `id_UNIQUE` (`id` ASC) VISIBLE,
  INDEX `fk_exhibitor_address1_idx` (`address_id` ASC) VISIBLE,
  CONSTRAINT `fk_exhibitor_address1`
    FOREIGN KEY (`address_id`)
    REFERENCES `vanfest`.`address` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `vanfest`.`terms`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `vanfest`.`terms` ;

CREATE TABLE IF NOT EXISTS `vanfest`.`terms` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `version` INT NOT NULL,
  `content` TEXT(2500) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `id_UNIQUE` (`id` ASC) VISIBLE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `vanfest`.`contract`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `vanfest`.`contract` ;

CREATE TABLE IF NOT EXISTS `vanfest`.`contract` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `date` DATE NOT NULL,
  `agreed` TINYINT NOT NULL,
  `people_in_party` INT NOT NULL,
  `social_posts` INT NOT NULL,
  `camp` TINYINT NOT NULL,
  `animal` TINYINT NOT NULL,
  `workshop` TINYINT NOT NULL,
  `selling_permits` TINYINT NOT NULL,
  `canopy` TINYINT NOT NULL,
  `map_number` CHAR(3) NULL,
  `exhibitor_id` INT NOT NULL,
  `terms_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `id_UNIQUE` (`id` ASC) VISIBLE,
  INDEX `fk_contract_exhibitor1_idx` (`exhibitor_id` ASC) VISIBLE,
  INDEX `fk_contract_terms1_idx` (`terms_id` ASC) VISIBLE,
  CONSTRAINT `fk_contract_exhibitor1`
    FOREIGN KEY (`exhibitor_id`)
    REFERENCES `vanfest`.`exhibitor` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_contract_terms1`
    FOREIGN KEY (`terms_id`)
    REFERENCES `vanfest`.`terms` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `vanfest`.`vehicle`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `vanfest`.`vehicle` ;

CREATE TABLE IF NOT EXISTS `vanfest`.`vehicle` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `make` VARCHAR(45) NOT NULL,
  `model` VARCHAR(45) NOT NULL,
  `dimensions` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE INDEX `id_UNIQUE` (`id` ASC) VISIBLE)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `vanfest`.`contract_has_vehicle`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `vanfest`.`contract_has_vehicle` ;

CREATE TABLE IF NOT EXISTS `vanfest`.`contract_has_vehicle` (
  `contract_id` INT NOT NULL,
  `vehicle_id` INT NOT NULL,
  `amount` INT NOT NULL,
  PRIMARY KEY (`contract_id`, `vehicle_id`),
  INDEX `fk_contract_has_vehicle_vehicle1_idx` (`vehicle_id` ASC) VISIBLE,
  INDEX `fk_contract_has_vehicle_contract1_idx` (`contract_id` ASC) VISIBLE,
  CONSTRAINT `fk_contract_has_vehicle_contract1`
    FOREIGN KEY (`contract_id`)
    REFERENCES `vanfest`.`contract` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_contract_has_vehicle_vehicle1`
    FOREIGN KEY (`vehicle_id`)
    REFERENCES `vanfest`.`vehicle` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;


-- --------------------------------------- END OF DATABASE CREATION ----------------------------------------------

USE vanfest;
-- BEGINNING OF INSERT STATEMENTS FOR VAN FEST DATABASE --------------------------------

-- ZIP INSERT
INSERT INTO zip(id,zip)
VALUES
(DEFAULT, 81201),
(DEFAULT, 94960),
(DEFAULT, 19135),
(DEFAULT, 29693),
(DEFAULT, 81211),
(DEFAULT, 15042),
(DEFAULT, 99515),
(DEFAULT, 28803),
(DEFAULT, 36083),
(DEFAULT, 56223),
(DEFAULT, 32615),
(DEFAULT, 34212),
(DEFAULT, 80470),
(DEFAULT, 60463),
(DEFAULT, 15801),
(DEFAULT, 58545),
(DEFAULT, 92591),
(DEFAULT, 76114),
(DEFAULT, 98672),
(DEFAULT, 77450),
(DEFAULT, 92570),
(DEFAULT, 27282),
(DEFAULT, 85044),
(DEFAULT, 85383),
(DEFAULT, 92127),
(DEFAULT, 30606),
(DEFAULT, 33142),
(DEFAULT, 93111);

SELECT *
FROM zip;

-- STATE INSERT
INSERT INTO state(id,state,zip_id)
VALUES
(DEFAULT,"CO",1),
(DEFAULT,"CA",2),
(DEFAULT,"CT",3),
(DEFAULT,"MA",4),
(DEFAULT,"PA",5),
(DEFAULT,"AK",6),
(DEFAULT,"NC",7),
(DEFAULT,"SC",8),
(DEFAULT,"AL",9),
(DEFAULT,"MN",10),
(DEFAULT,"FL",11),
(DEFAULT,"IL",12),
(DEFAULT,"ND",13),
(DEFAULT,"TX",14),
(DEFAULT,"WA",15),
(DEFAULT,"AZ",16),
(DEFAULT,"GA",17),
(DEFAULT,"MI",18),
(DEFAULT,"MO",19),
(DEFAULT,"UT",20),
(DEFAULT,"NJ",21),
(DEFAULT,"OH",22),
(DEFAULT,"OR",23),
(DEFAULT,"NV",24),
(DEFAULT,"NY",25),
(DEFAULT,"SD",26),
(DEFAULT,"WI",27),
(DEFAULT,"IN",28);

SELECT *
FROM state;


-- CITY INSERT
INSERT INTO city(id,city,state_id)
VALUES
(DEFAULT,"Salida",1),
(DEFAULT,"San Anselmo",2),
(DEFAULT,"East Hartford",3),
(DEFAULT,"Beverly",4),
(DEFAULT,"Fresno",5),
(DEFAULT,"San Marcos",6),
(DEFAULT,"Buena Vista",7),
(DEFAULT,"Freedom",8),
(DEFAULT,"Anchorage",9),
(DEFAULT,"Asheville",10),
(DEFAULT,"Westminster",11),
(DEFAULT,"Tuskegee",12),
(DEFAULT,"Clarkfield",13),
(DEFAULT,"Alachua",14),
(DEFAULT,"Bradenton",15),
(DEFAULT,"Pine",16),
(DEFAULT,"Oak lawn",17),
(DEFAULT,"DuBois",18),
(DEFAULT,"Hazen",19),
(DEFAULT,"Temecula",20),
(DEFAULT,"Fort worth",21),
(DEFAULT,"White salmon",22),
(DEFAULT,"Katy",23),
(DEFAULT,"Perris",24),
(DEFAULT,"Jamestown",25),
(DEFAULT,"Phoenix",26),
(DEFAULT,"Peoria",27),
(DEFAULT,"San Diego",28);

SELECT *
FROM city;

-- ADDRESS INSERT
INSERT INTO address(id,address_1,address_2,city_id)
VALUES
(DEFAULT,"PO BOX 1037",NULL,1),
(DEFAULT,"1513 San Anselmo Ave",NULL,2),
(DEFAULT,"246 Burke St",NULL,3),
(DEFAULT,"95 Grover St",NULL,4),
(DEFAULT,"8072 N MILLBROOK AVE","APT 202",5),
(DEFAULT,"1005 Armorlite Dr","apt 229",6),
(DEFAULT,"PO Box 5176",NULL,7),
(DEFAULT,"145 ridgewood drive",NULL,8),
(DEFAULT,"1160 China Berry Circle",NULL,9),
(DEFAULT,"11 Alta Ave",NULL,10),
(DEFAULT,"390 River Rd",NULL,11),
(DEFAULT,"1008 Southeast Bluewater Court",NULL,12),
(DEFAULT,"2153 410th St",NULL,13),
(DEFAULT,"17710 NW 175Th Ave",NULL,14),
(DEFAULT,"815 137th St E",NULL,15),
(DEFAULT,"105 Pima Trail",NULL,16),
(DEFAULT,"8849 Nashville Ave",NULL,17),
(DEFAULT,"829 Maple Ave",NULL,18),
(DEFAULT,"312 8th ave nw",NULL,19),
(DEFAULT,"40724 Avenida Centenario",NULL,20),
(DEFAULT,"5403 flagstone dr",NULL,21),
(DEFAULT,"54 Catherine creek rd",NULL,22),
(DEFAULT,"2153 410th st",NULL,23),
(DEFAULT,"1909 Sand Hollow LN",NULL,24),
(DEFAULT,"27260 peach st",NULL,25),
(DEFAULT,"3035 Sherrill Ave",NULL,26),
(DEFAULT,"5132 E Nambe St",NULL,27),
(DEFAULT,"13412 W Jesse Red Dr",NULL,28);

SELECT *
FROM address;

-- EXHIBITOR INSERT
INSERT INTO exhibitor(id,first_name,last_name,business_name,social_handle,email,phone,address_id)
VALUES
(DEFAULT,"Clayton","Valentine",NULL,"@ClaytonValentine","ClaytonValentine@gmail.com","2988292220",1),
(DEFAULT,"Damaris","Stephenson",NULL,"@DamarisStephenson","DamarisStephenson@gmail.com","4239231485",2),
(DEFAULT,"Kristina","Rush",NULL,"@KristinaRush","KristinaRush@gmail.com","5474970619",3),
(DEFAULT,"Jaylene","Jones",NULL,"@JayleneJones","JayleneJones@gmail.com","7596609677",4),
(DEFAULT,"Heather","Zamora","Golden Advantures","@HeatherZamora","HeatherZamora@gmail.com","4149211321",5),
(DEFAULT,"Santino","Matthews",NULL,"@SantinoMatthews","SantinoMatthews@gmail.com","5727399311",6),
(DEFAULT,"Simeon","Ferrell",NULL,"@SimeonFerrell","SimeonFerrell@gmail.com","9176697152",7),
(DEFAULT,"Jaden","Dougherty",NULL,"@JadenDougherty","JadenDougherty@gmail.com","3989823656",8),
(DEFAULT,"Connor","Rangel",NULL,"@ConnorRangel","ConnorRangel@gmail.com","5259716441",9),
(DEFAULT,"Rowan","Lynn","BooneDock Vans","@RowanLynn","RowanLynn@gmail.com","4572929105",10),
(DEFAULT,"Miguel","Tanner",NULL,"@MiguelTanner","MiguelTanner@gmail.com","3577954994",11),
(DEFAULT,"Aidan","Savage",NULL,"@AidanSavage","AidanSavage@gmail.com","6413040753",12),
(DEFAULT,"Bronson","Rocha",NULL,"@BronsonRocha","BronsonRocha@gmail.com","9373253750",13),
(DEFAULT,"Marissa","Myers","Holdfast Carving","@MarissaMyers","MarissaMyers@gmail.com","2367804051",14),
(DEFAULT,"Zayden","Allison","RPK Customs","@ZaydenAllison","ZaydenAllison@gmail.com","7206222665",15),
(DEFAULT,"Kody","Daniel",NULL,"@KodyDaniel","KodyDaniel@gmail.com","8713664009",16),
(DEFAULT,"Owen","Erickson",NULL,"@OwenErickson","OwenErickson@gmail.com","6779659291",17),
(DEFAULT,"Ahmed","Esparza",NULL,"@AhmedEsparza","AhmedEsparza@gmail.com","6883218896",18),
(DEFAULT,"Skyler","Haas","Outsidevibes","@SkylerHaas","SkylerHaas@gmail.com","3643414117",19),
(DEFAULT,"Wendy","Schroeder",NULL,"@WendySchroeder","WendySchroeder@gmail.com","6019104885",20),
(DEFAULT,"Amber","Bridges","Earthbuds","@AmberBridges","AmberBridges@gmail.com","5708399594",21),
(DEFAULT,"Jayvon","Villarreal",NULL,"@JayvonVillarreal","JayvonVillarreal@gmail.com","5445972604",22),
(DEFAULT,"Zander","Brewer","Modern Vans","@ZanderBrewer","ZanderBrewer@gmail.com","5354587375",23),
(DEFAULT,"Brice","Payne","Eldrich Solutions","@BricePayne","BricePayne@gmail.com","8232561400",24),
(DEFAULT,"Simeon","Rhodes",NULL,"@SimeonRhodes","SimeonRhodes@gmail.com","6974534206",25),
(DEFAULT,"Jamarion","Molina","Quirks & Conjure","@JamarionMolina","JamarionMolina@gmail.com","3868223370",26),
(DEFAULT,"Gaige","Pratt",NULL,"@GaigePratt","GaigePratt@gmail.com","4983204016",27),
(DEFAULT,"Rylee","House",NULL,"@RyleeHouse","RyleeHouse@gmail.com","9279866058",28);

SELECT *
FROM exhibitor;

-- TERMS INSERT
INSERT INTO terms(id,version,content)
VALUES(DEFAULT,"1.0","RULES AND AGREEMENTS OF THE FESTIVAL
We are a Van Life Festival; therefore, products and services must be related to camper/sprinter vans and/or off-road camping adventure gear. Other items will be allowed upon written permission only.
All vendors/exhibitors are responsible and liable for any damage that may come to their property (vans, merchandise, products, etc.) during the event.
All vendors are responsible for liability insurance and compliance with any and all requirements of State of Utah.
All vehicles must be displayed in a clean and safe manner.
By signing this you allow attendees to view the inside and outside of your vehicle with your supervision.
Vendors/Exhibitors are responsible for leaving a clean area at closing.
All exhibitors shall exhibit in professional manners always.
Tables and canopies are allowed, based upon prior approval.
Spaces shall be assigned by the Festival Director.
All dogs/pets must be leashed or contained at the event.
I/We agree to abide by all rules and regulations as mentioned.");

select *
FROM terms;

-- VEHICLE INSERT
INSERT INTO vehicle(id,make,model,dimensions)
VALUES
(DEFAULT,"RAM","Promaster 1200","20x6x10"),
(DEFAULT,"RAM","Promaster 1400","24x6x10"),
(DEFAULT,"Mercedes","Sprinter 180","22x7.5x9");

SELECT *
FROM vehicle;

-- CONTRACT INSERT
INSERT INTO contract (id,date,agreed,people_in_party,social_posts,camp,animal,workshop,selling_permits,canopy,map_number,exhibitor_id,terms_id)
VALUES
(DEFAULT,"2020/03/17",1,6,15,1,1,0,0,0,"24",1,1),
(DEFAULT,"2020/03/18",1,2,1,1,1,1,1,1,"26",2,1),
(DEFAULT,"2020/03/19",1,1,2,1,0,0,1,0,"27",3,1),
(DEFAULT,"2020/03/17",1,2,10,1,0,0,0,0,"29",4,1),
(DEFAULT,"2020/03/18",1,2,8,1,1,0,0,0,"30",5,1),
(DEFAULT,"2020/03/19",1,3,13,1,0,0,0,1,"31",6,1),
(DEFAULT,"2020/03/17",1,2,14,1,1,0,0,1,"32",7,1),
(DEFAULT,"2020/03/18",1,2,11,1,0,0,0,1,"33",8,1),
(DEFAULT,"2020/03/19",1,4,4,1,0,0,0,0,"34",9,1),
(DEFAULT,"2020/03/17",1,2,5,1,1,0,1,1,"36",10,1),
(DEFAULT,"2020/03/18",1,2,6,1,0,0,1,1,"37",11,1),
(DEFAULT,"2020/03/19",1,2,12,1,1,0,1,0,"38",12,1),
(DEFAULT,"2020/03/17",1,1,12,1,0,0,0,0,"39",13,1),
(DEFAULT,"2020/03/18",1,1,11,1,1,0,1,1,"40",14,1),
(DEFAULT,"2020/03/19",1,1,3,1,1,1,0,0,"41",15,1),
(DEFAULT,"2020/03/17",1,2,3,1,1,0,0,1,"42",16,1),
(DEFAULT,"2020/03/18",1,2,3,1,1,0,0,1,"43",17,1),
(DEFAULT,"2020/03/19",1,3,8,1,1,0,0,0,"44",18,1),
(DEFAULT,"2020/03/17",1,1,10,1,0,1,0,0,"45",19,1),
(DEFAULT,"2020/03/18",1,2,1,1,0,1,0,0,"47",20,1),
(DEFAULT,"2020/03/19",1,2,12,1,1,0,1,0,"48",21,1),
(DEFAULT,"2020/03/17",1,5,13,1,0,0,0,1,"49",22,1),
(DEFAULT,"2020/03/18",1,1,1,1,0,0,0,0,"50",23,1),
(DEFAULT,"2020/03/19",1,1,6,1,1,0,0,1,"51",24,1),
(DEFAULT,"2020/03/17",1,2,9,1,1,1,0,0,"52",25,1),
(DEFAULT,"2020/03/18",1,2,8,1,1,1,1,1,"53",26,1),
(DEFAULT,"2020/03/19",1,2,0,1,1,1,0,0,"55",27,1),
(DEFAULT,"2020/03/17",1,2,13,1,1,1,1,1,"57",28,1);

SELECT *
FROM contract;

-- CONTRACT_HAS_VEHICLE INSERT
INSERT INTO contract_has_vehicle(contract_id,vehicle_id,amount)
VALUES
(1,1,1),
(2,2,1),
(3,2,1),
(4,3,1),
(5,3,1),
(6,3,1),
(7,2,1),
(8,3,1),
(9,1,1),
(10,2,1),
(11,2,1),
(12,2,1),
(13,1,1),
(14,3,1),
(15,2,1),
(16,1,2),
(17,2,1),
(18,3,1),
(19,3,1),
(20,1,1),
(21,2,1),
(22,2,1),
(23,1,1),
(24,1,1),
(25,3,1),
(26,3,1),
(27,2,1),
(28,1,1);

SELECT *
FROM contract_has_vehicle;


## Here are the create, retrieves, updates, and deletes

USE vanfest;

-- Drop the sp delete films procedure
DROP PROCEDURE IF EXISTS sp_delete_exhibitor;
DROP PROCEDURE IF EXISTS sp_create_exhibitor;
DROP PROCEDURE IF EXISTS sp_update_exhibitor;

-- Fixing the Foreign Key Constraints
-- Address Table
ALTER TABLE `vanfest`.`address` 
DROP FOREIGN KEY `fk_address_city1`;
ALTER TABLE `vanfest`.`address` 
ADD CONSTRAINT `fk_address_city1`
  FOREIGN KEY (`city_id`)
  REFERENCES `vanfest`.`city` (`id`)
  ON DELETE NO ACTION
  ON UPDATE NO ACTION;


-- City Table
ALTER TABLE `vanfest`.`city` 
DROP FOREIGN KEY `fk_city_state1`;
ALTER TABLE `vanfest`.`city` 
ADD CONSTRAINT `fk_city_state1`
  FOREIGN KEY (`state_id`)
  REFERENCES `vanfest`.`state` (`id`)
  ON DELETE NO ACTION
  ON UPDATE NO ACTION;

  
-- Contract Table
ALTER TABLE `vanfest`.`contract` 
DROP FOREIGN KEY `fk_contract_exhibitor1`;
ALTER TABLE `vanfest`.`contract` 
ADD CONSTRAINT `fk_contract_exhibitor1`
  FOREIGN KEY (`exhibitor_id`)
  REFERENCES `vanfest`.`exhibitor` (`id`)
  ON DELETE NO ACTION
  ON UPDATE NO ACTION;
  
-- Contract Has Vehicles Table
ALTER TABLE `vanfest`.`contract_has_vehicle` 
DROP FOREIGN KEY `fk_contract_has_vehicle_contract1`;
ALTER TABLE `vanfest`.`contract_has_vehicle` 
ADD CONSTRAINT `fk_contract_has_vehicle_contract1`
  FOREIGN KEY (`contract_id`)
  REFERENCES `vanfest`.`contract` (`id`)
  ON DELETE NO ACTION
  ON UPDATE NO ACTION;

-- Exhibitor Table
ALTER TABLE `vanfest`.`exhibitor` 
DROP FOREIGN KEY `fk_exhibitor_address1`;
ALTER TABLE `vanfest`.`exhibitor` 
ADD CONSTRAINT `fk_exhibitor_address1`
  FOREIGN KEY (`address_id`)
  REFERENCES `vanfest`.`address` (`id`)
  ON DELETE NO ACTION
  ON UPDATE NO ACTION;
  
-- State Table
ALTER TABLE `vanfest`.`state` 
DROP FOREIGN KEY `fk_state_zip`;
ALTER TABLE `vanfest`.`state` 
ADD CONSTRAINT `fk_state_zip`
  FOREIGN KEY (`zip_id`)
  REFERENCES `vanfest`.`zip` (`id`)
  ON DELETE NO ACTION
  ON UPDATE NO ACTION;



-- Stored Procedure to Delete Exhibitors
DELIMITER //

CREATE PROCEDURE sp_delete_exhibitor
(
IN id_param INT(3)
)

BEGIN

	DECLARE sql_error TINYINT DEFAULT FALSE;
    
    DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
		SET sql_error = TRUE;
	
START TRANSACTION;
        

-- Create deletes to delete everywhere the exhibitor was found

DELETE FROM contract_has_vehicle
WHERE contract_id = (SELECT id
						FROM contract
                        WHERE exhibitor_id = id_param);


DELETE FROM contract
WHERE exhibitor_id = id_param;


DELETE FROM exhibitor
WHERE id = id_param;

        
        
        IF sql_error = FALSE THEN
			COMMIT;
            SELECT "Exhibitors successfully deleted";
		ELSE
			ROLLBACK;
            SELECT "ERROR: Exhibitors cannot be deleted";
		END IF;

END //


-- Stored Procedure to Create Exhibitors
DELIMITER //

CREATE PROCEDURE sp_create_exhibitor
(
IN first_name_param VARCHAR(25),
IN last_name_param VARCHAR(25),
IN business_name_param VARCHAR(25),
IN social_handle_param VARCHAR(25),
IN email_param VARCHAR(25),
IN phone_param CHAR(10),
IN address_id_param INT(3)
)

BEGIN

	DECLARE sql_error TINYINT DEFAULT FALSE;
    
    DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
		SET sql_error = TRUE;
	
START TRANSACTION;
        

-- Create the new exhibitor

INSERT INTO exhibitor
	VALUES(DEFAULT,first_name_param,last_name_param,business_name_param,social_handle_param,email_param,phone_param,address_id_param);

        
        
        IF sql_error = FALSE THEN
			COMMIT;
            SELECT "Exhibitors successfully created";
		ELSE
			ROLLBACK;
            SELECT "ERROR: Exhibitors cannot be created";
		END IF;

END //


-- Stored Procedure to Update Exhibitors
DELIMITER //

CREATE PROCEDURE sp_update_exhibitor
(
IN first_name_param VARCHAR(25),
IN last_name_param VARCHAR(25),
IN id_param INT(3)
)

BEGIN

	DECLARE sql_error TINYINT DEFAULT FALSE;
    
    DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
		SET sql_error = TRUE;
	
START TRANSACTION;
        

-- Create the new exhibitor

UPDATE exhibitor
	SET first_name = first_name_param, last_name = last_name_param
    WHERE id = id_param;

        
        
        IF sql_error = FALSE THEN
			COMMIT;
            SELECT "Exhibitors successfully updated";
		ELSE
			ROLLBACK;
            SELECT "ERROR: Exhibitors cannot be updated";
		END IF;

END //


-- Calling Stored Procedures
CALL sp_delete_exhibitor(1);
CALL sp_create_exhibitor("Ashton", "Glines", "Ashton's Business", "@ashtonglines", "ashtong@gmail.com", "4352435643",3);
CALL sp_update_exhibitor("Rebecca", "Wirthlin",29);


-- SELECTING EVERYTHING FROM THE EXHIBITOR TABLE TO VIEW THE CHANGES.

SELECT *
FROM exhibitor;



