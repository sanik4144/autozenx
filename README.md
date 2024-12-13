-- phpMyAdmin SQL Dump
-- version 5.2.1
-- https://www.phpmyadmin.net/
--
-- Host: 127.0.0.1
-- Generation Time: Dec 13, 2024 at 06:35 AM
-- Server version: 10.4.32-MariaDB
-- PHP Version: 8.2.12

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
START TRANSACTION;
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Database: `autozenx`
--

DELIMITER $$
--
-- Procedures
--
CREATE DEFINER=`root`@`localhost` PROCEDURE `car_review` (IN `productid` INT)   BEGIN
    SELECT product.Name, product.Description, reviews.Comment, reviews.Date
    FROM product
    JOIN reviews ON reviews.ProductID = product.ProductID
    WHERE reviews.ProductID = productid;
END$$

CREATE DEFINER=`root`@`localhost` PROCEDURE `GetBuyerWishlistDetails` (IN `inputBuyerID` INT)   BEGIN
    SELECT 
        buyer.Name AS BuyerName,
        buyer.Email,
        product.Name AS ProductName,
        product.Description,
        product.Status
    FROM 
        wishlist
    JOIN 
        buyer ON wishlist.BuyerID = buyer.BuyerID
    JOIN 
        product On  wishlist.ProductID = product.ProductID
    WHERE 
        buyer.BuyerID = inputBuyerID;
END$$

CREATE DEFINER=`root`@`localhost` PROCEDURE `sell` (IN `productid` INT)   BEGIN
    UPDATE product
    SET product.Status = 'Sold'
    WHERE product.ProductID = productid;
END$$

DELIMITER ;

-- --------------------------------------------------------

--
-- Table structure for table `admin`
--

CREATE TABLE `admin` (
  `AdminID` int(11) NOT NULL,
  `Name` varchar(100) DEFAULT NULL,
  `Email` varchar(100) DEFAULT NULL,
  `PasswordHash` varchar(255) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `admin`
--

INSERT INTO `admin` (`AdminID`, `Name`, `Email`, `PasswordHash`) VALUES
(1, 'Admin1', 'admin1@example.com', 'hashed_password_1'),
(2, 'Admin2', 'admin2@example.com', 'hashed_password_2'),
(3, 'Admin3', 'admin3@example.com', 'hashed_password_3'),
(4, 'Admin4', 'admin4@example.com', 'hashed_password_4'),
(5, 'Admin5', 'admin5@example.com', 'hashed_password_5');

-- --------------------------------------------------------

--
-- Table structure for table `buyer`
--

CREATE TABLE `buyer` (
  `BuyerID` int(11) NOT NULL,
  `Name` varchar(100) DEFAULT NULL,
  `Email` varchar(100) DEFAULT NULL,
  `Phone` varchar(15) DEFAULT NULL,
  `Address` varchar(255) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `buyer`
--

INSERT INTO `buyer` (`BuyerID`, `Name`, `Email`, `Phone`, `Address`) VALUES
(1, 'Alice Johnson', 'alice@example.com', '1234567890', '123 Maple St'),
(2, 'Bob Smith', 'bob@example.com', '0987654321', '456 Elm St'),
(3, 'Charlie Brown', 'charlie@example.com', '1122334455', '789 Oak St'),
(4, 'Diana Ross', 'diana@example.com', '6677889900', '321 Pine St'),
(5, 'Eve Taylor', 'eve@example.com', '5566778899', '654 Spruce St');

-- --------------------------------------------------------

--
-- Stand-in structure for view `buyerpurchasehistory`
-- (See below for the actual view)
--
CREATE TABLE `buyerpurchasehistory` (
`BuyerID` int(11)
,`BuyerName` varchar(100)
,`CarName` varchar(100)
,`CarPrice` decimal(10,2)
,`TransactionDate` timestamp
,`TransactionStatus` enum('Pending','Completed','Cancelled')
);

-- --------------------------------------------------------

--
-- Table structure for table `car_paper_verification`
--

CREATE TABLE `car_paper_verification` (
  `VerificationID` int(11) NOT NULL,
  `ProductID` int(11) DEFAULT NULL,
  `VerificationStatus` enum('Pending','Verified','Rejected') DEFAULT NULL,
  `DateVerified` timestamp NOT NULL DEFAULT current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `car_paper_verification`
--

INSERT INTO `car_paper_verification` (`VerificationID`, `ProductID`, `VerificationStatus`, `DateVerified`) VALUES
(1, 1, 'Verified', '2024-11-24 16:09:02'),
(2, 2, 'Pending', '2024-11-24 16:09:02'),
(3, 3, 'Verified', '2024-11-24 16:09:02'),
(4, 4, 'Rejected', '2024-11-24 16:09:02'),
(5, 5, 'Pending', '2024-11-24 16:09:02'),
(6, 33, 'Pending', '2024-12-09 04:41:12'),
(7, 34, 'Pending', '2024-12-09 04:43:24'),
(8, 35, 'Pending', '2024-12-09 04:44:54'),
(9, 36, 'Pending', '2024-12-09 04:44:54'),
(10, 37, 'Pending', '2024-12-09 04:44:54'),
(11, 38, 'Pending', '2024-12-09 04:44:54'),
(12, 39, 'Pending', '2024-12-09 04:44:54'),
(13, 40, 'Pending', '2024-12-09 04:44:54'),
(14, 41, 'Pending', '2024-12-09 04:44:54');

-- --------------------------------------------------------

--
-- Table structure for table `faq`
--

CREATE TABLE `faq` (
  `FAQID` int(11) NOT NULL,
  `Question` text DEFAULT NULL,
  `Answer` text DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `faq`
--

INSERT INTO `faq` (`FAQID`, `Question`, `Answer`) VALUES
(1, 'What is the return policy?', 'You can request a replacement within 3 days.'),
(2, 'How is the payment handled?', 'Payments are processed securely online.'),
(3, 'Are cars inspected before listing?', 'Yes, we verify car papers and inspect cars.'),
(4, 'What if I find an issue after purchase?', 'You can report issues or request a replacement.'),
(5, 'How can I contact customer service?', 'You can email us at support@autozenx.com.');

-- --------------------------------------------------------

--
-- Table structure for table `highest_priced_car`
--

CREATE TABLE `highest_priced_car` (
  `RecordID` int(11) NOT NULL,
  `ProductID` int(11) DEFAULT NULL,
  `CarName` varchar(100) DEFAULT NULL,
  `Price` decimal(10,2) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `highest_priced_car`
--

INSERT INTO `highest_priced_car` (`RecordID`, `ProductID`, `CarName`, `Price`) VALUES
(1, 2, 'Honda Civic', 6000.00),
(2, 1, 'Toyota Corolla', 5800.00),
(3, 4, 'Chevrolet Malibu', 5700.00),
(4, 5, 'Nissan Altima', 5500.00),
(5, 3, 'Ford Focus', 5400.00);

-- --------------------------------------------------------

--
-- Table structure for table `lowest_priced_car`
--

CREATE TABLE `lowest_priced_car` (
  `RecordID` int(11) NOT NULL,
  `ProductID` int(11) DEFAULT NULL,
  `CarName` varchar(100) DEFAULT NULL,
  `Price` decimal(10,2) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `lowest_priced_car`
--

INSERT INTO `lowest_priced_car` (`RecordID`, `ProductID`, `CarName`, `Price`) VALUES
(1, 3, 'Ford Focus', 4000.00),
(2, 5, 'Nissan Altima', 4200.00),
(3, 1, 'Toyota Corolla', 4500.00),
(4, 2, 'Honda Civic', 4800.00),
(5, 4, 'Chevrolet Malibu', 4900.00);

-- --------------------------------------------------------

--
-- Table structure for table `maintenance_cost`
--

CREATE TABLE `maintenance_cost` (
  `MaintenanceID` int(11) NOT NULL,
  `ProductID` int(11) DEFAULT NULL,
  `CarName` varchar(100) DEFAULT NULL,
  `ServiceType` varchar(100) DEFAULT NULL,
  `Cost` decimal(10,2) DEFAULT NULL,
  `ServiceDate` date DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `maintenance_cost`
--

INSERT INTO `maintenance_cost` (`MaintenanceID`, `ProductID`, `CarName`, `ServiceType`, `Cost`, `ServiceDate`) VALUES
(1, 1, 'Toyota Corolla', 'Oil Change', 75.00, '2024-11-01'),
(2, 2, 'Honda Civic', 'Tire Replacement', 150.00, '2024-11-02'),
(3, 3, 'Ford Focus', 'Brake Pad Replacement', 200.00, '2024-11-03'),
(4, 4, 'Chevrolet Malibu', 'Engine Tuning', 300.00, '2024-11-04'),
(5, 5, 'Nissan Altima', 'Battery Replacement', 120.00, '2024-11-05');

-- --------------------------------------------------------

--
-- Stand-in structure for view `mostsearchedcars`
-- (See below for the actual view)
--
CREATE TABLE `mostsearchedcars` (
`CarName` varchar(100)
,`SearchCount` int(11)
);

-- --------------------------------------------------------

--
-- Table structure for table `most_searched_car`
--

CREATE TABLE `most_searched_car` (
  `RecordID` int(11) NOT NULL,
  `CarName` varchar(100) DEFAULT NULL,
  `SearchCount` int(11) DEFAULT NULL,
  `LastSearched` timestamp NOT NULL DEFAULT current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `most_searched_car`
--

INSERT INTO `most_searched_car` (`RecordID`, `CarName`, `SearchCount`, `LastSearched`) VALUES
(1, 'Toyota Corolla', 150, '2024-11-24 16:06:23'),
(2, 'Honda Civic', 120, '2024-11-24 16:06:23'),
(3, 'Ford Focus', 100, '2024-11-24 16:06:23'),
(4, 'Chevrolet Malibu', 90, '2024-11-24 16:06:23'),
(5, 'Nissan Altima', 80, '2024-11-24 16:06:23');

-- --------------------------------------------------------

--
-- Table structure for table `out_of_stock_cars`
--

CREATE TABLE `out_of_stock_cars` (
  `RecordID` int(11) NOT NULL,
  `ProductID` int(11) DEFAULT NULL,
  `CarName` varchar(100) DEFAULT NULL,
  `DateOutOfStock` timestamp NOT NULL DEFAULT current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `out_of_stock_cars`
--

INSERT INTO `out_of_stock_cars` (`RecordID`, `ProductID`, `CarName`, `DateOutOfStock`) VALUES
(1, 1, 'Toyota Corolla', '2024-11-05 18:00:00'),
(2, 2, 'Honda Civic', '2024-11-06 18:00:00'),
(3, 3, 'Ford Focus', '2024-11-07 18:00:00'),
(4, 4, 'Chevrolet Malibu', '2024-11-08 18:00:00'),
(5, 5, 'Nissan Altima', '2024-11-09 18:00:00');

-- --------------------------------------------------------

--
-- Table structure for table `product`
--

CREATE TABLE `product` (
  `ProductID` int(11) NOT NULL,
  `SellerID` int(11) DEFAULT NULL,
  `Name` varchar(100) DEFAULT NULL,
  `Price` decimal(10,2) DEFAULT NULL,
  `Description` text DEFAULT NULL,
  `Status` enum('Available','Sold') DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `product`
--

INSERT INTO `product` (`ProductID`, `SellerID`, `Name`, `Price`, `Description`, `Status`) VALUES
(1, 1, 'Toyota Corolla', 5000.00, '2010 model, excellent condition', 'Sold'),
(2, 2, 'Honda Civic', 6000.00, '2012 model, well-maintained', 'Sold'),
(3, 3, 'Ford Focus', 4000.00, '2011 model, minor scratches', 'Sold'),
(4, 4, 'Chevrolet Malibu', 3500.00, '2013 model, fully serviced', 'Sold'),
(5, 5, 'Nissan Altima', 5200.00, '2014 model, great mileage', 'Sold'),
(6, 1, 'Toyota XCorolla', 5050.00, '2010 model, excellent condition', 'Sold'),
(7, 2, 'Honda Ultima', 6060.00, '2012 model, well-maintained', 'Sold'),
(13, 1, 'Hyundai Elantra', 5200.00, 'Stylish sedan with advanced safety features', 'Sold'),
(14, 2, 'BMW 3 Series', 25000.00, 'Luxury compact car with sporty performance', 'Sold'),
(15, 3, 'Tesla Model 3', 35000.00, 'Electric car with impressive range and performance', 'Available'),
(16, 2, 'Jeep Wrangler', 28000.00, 'Off-road SUV with rugged capabilities', 'Sold'),
(17, 5, 'Ford Mustang', 32000.00, 'Classic muscle car with powerful engine', 'Sold'),
(18, 1, 'Toyota Corolla_pro', 5000.00, '2010 model, excellent condition', 'Sold'),
(19, 2, 'Honda Civic_cruise', 6000.00, '2012 model, well-maintained', 'Sold'),
(20, 3, 'Ford Focus', 4000.00, '2011 model, minor scratches', 'Sold'),
(21, 4, 'Chevrolet Malibu', 5500.00, '2013 model, fully serviced', 'Sold'),
(22, 5, 'Nissan Ultima', 5200.00, '2014 model, great mileage', 'Available'),
(23, 1, 'Toyota XCorolla2', 5050.00, '2010 model, excellent condition', 'Available'),
(24, 2, 'Honda Ultima_pro', 6060.00, '2012 model, well-maintained', 'Available'),
(25, 1, 'Toyota Corolla_pro', 5000.00, '2010 model, excellent condition', 'Available'),
(26, 2, 'Honda Civic_cruise', 6000.00, '2012 model, well-maintained', 'Available'),
(27, 3, 'Ford Focus', 4000.00, '2011 model, minor scratches', 'Available'),
(28, 4, 'Chevrolet Malibu', 5500.00, '2013 model, fully serviced', 'Available'),
(29, 5, 'Nissan Ultima', 5200.00, '2014 model, great mileage', 'Available'),
(30, 1, 'Toyota XCorolla2', 5050.00, '2010 model, excellent condition', 'Available'),
(31, 2, 'Honda Ultima_pro', 6060.00, '2012 model, well-maintained', 'Available'),
(33, 4, 'New Ultima_pro', 6060.00, '2024 model, well-maintained', 'Available'),
(34, 5, 'Hwunai Ultima_pro', 60600.00, '2012 Primo, well-maintained', 'Available'),
(35, 1, 'Toyota Corolla_pro', 5000.00, '2010 model, excellent condition', 'Available'),
(36, 2, 'Honda Civic_cruise', 6000.00, '2012 model, well-maintained', 'Available'),
(37, 3, 'Ford Focus', 4000.00, '2011 model, minor scratches', 'Available'),
(38, 4, 'Chevrolet Malibu', 5500.00, '2013 model, fully serviced', 'Available'),
(39, 5, 'Nissan Ultima', 5200.00, '2014 model, great mileage', 'Available'),
(40, 1, 'Toyota XCorolla2', 5050.00, '2010 model, excellent condition', 'Available'),
(41, 2, 'Honda Ultima_pro', 6060.00, '2012 model, well-maintained', 'Available'),
(42, 1, 'Toyota Corolla_pro', 5000.00, '2010 model, excellent condition', 'Available'),
(43, 2, 'Honda Civic_cruise', 6000.00, '2012 model, well-maintained', 'Available'),
(44, 3, 'Ford Focus', 4000.00, '2011 model, minor scratches', 'Available'),
(45, 4, 'Chevrolet Malibu', 5500.00, '2013 model, fully serviced', 'Available'),
(46, 5, 'Nissan Ultima', 5200.00, '2014 model, great mileage', 'Available'),
(47, 1, 'Toyota XCorolla2', 5050.00, '2010 model, excellent condition', 'Available'),
(48, 2, 'Honda Ultima_pro', 6060.00, '2012 model, well-maintained', 'Available'),
(49, 1, 'Toyota Corolla_pro', 5000.00, '2010 model, excellent condition', 'Available'),
(50, 2, 'Honda Civic_cruise', 6000.00, '2012 model, well-maintained', 'Available'),
(51, 3, 'Ford Focus', 4000.00, '2011 model, minor scratches', 'Available'),
(52, 1, 'Toyota Corolla_pro', 5000.00, '2010 model, excellent condition', 'Available'),
(53, 2, 'Honda Civic_cruise', 6000.00, '2012 model, well-maintained', 'Available'),
(54, 3, 'Ford Focus', 4000.00, '2011 model, minor scratches', 'Available');

--
-- Triggers `product`
--
DELIMITER $$
CREATE TRIGGER `after_product_insert` AFTER INSERT ON `product` FOR EACH ROW BEGIN
    -- Insert a new record into car_paper_verification with 'Pending' status
    INSERT INTO car_paper_verification (productid, verificationstatus)
    VALUES (NEW.productid, 'Pending');
END
$$
DELIMITER ;
DELIMITER $$
CREATE TRIGGER `after_product_sold` AFTER UPDATE ON `product` FOR EACH ROW BEGIN
    -- Check if the product status has changed to 'Sold'
    IF NEW.Status = 'Sold' AND OLD.Status = 'Available' THEN
        -- Insert into sold_car directly with a subquery to find TransactionID
        INSERT INTO sold_car (ProductID, TransactionID, Name, Model)
        SELECT 
            NEW.ProductID, 
            TransactionID, 
            NEW.Name, 
            NEW.Description
        FROM transaction
        WHERE ProductID = NEW.ProductID
        LIMIT 1;
    END IF;
END
$$
DELIMITER ;
DELIMITER $$
CREATE TRIGGER `after_status_update` AFTER UPDATE ON `product` FOR EACH ROW BEGIN
    -- Check if the status is updated to 'Sold'
    IF NEW.Status = 'Sold' AND OLD.Status = 'Available' THEN
        -- Check if the seller already has an entry in the sales_report
        IF EXISTS (
            SELECT 1 
            FROM sales_report 
            WHERE sellerID = NEW.sellerID
        ) THEN
            -- Update the existing record in sales_report
            UPDATE sales_report
            SET 
                numberofcarssold = numberofcarssold + 1,
                reportdate = CURDATE()
            WHERE sellerID = NEW.sellerID;
        ELSE
            -- Insert a new record into sales_report
            INSERT INTO sales_report (sellerID, numberofcarssold, reportdate)
            VALUES (NEW.sellerID, 1, CURDATE());
        END IF;
    END IF;
END
$$
DELIMITER ;
DELIMITER $$
CREATE TRIGGER `prevent_duplicate_sale` BEFORE UPDATE ON `product` FOR EACH ROW BEGIN
    IF OLD.Status = 'Sold' AND NEW.Status = 'Available' THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'Cannot change status of a sold product back to available';
    END IF;
END
$$
DELIMITER ;

-- --------------------------------------------------------

--
-- Table structure for table `refund_request`
--

CREATE TABLE `refund_request` (
  `RefundID` int(11) NOT NULL,
  `TransactionID` int(11) DEFAULT NULL,
  `Reason` text DEFAULT NULL,
  `DateRequested` timestamp NOT NULL DEFAULT current_timestamp(),
  `Status` enum('Pending','Approved','Rejected') DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `refund_request`
--

INSERT INTO `refund_request` (`RefundID`, `TransactionID`, `Reason`, `DateRequested`, `Status`) VALUES
(1, 1, 'Did not match expectations.', '2024-11-24 16:16:36', 'Approved'),
(2, 2, 'Minor damage found.', '2024-11-24 16:16:36', 'Rejected'),
(3, 3, 'Issue with battery.', '2024-11-24 16:16:36', 'Pending'),
(4, 4, 'Tires are worn out.', '2024-11-24 16:16:36', 'Pending'),
(5, 5, 'Engine issues.', '2024-11-24 16:16:36', 'Approved');

-- --------------------------------------------------------

--
-- Table structure for table `replacement_request`
--

CREATE TABLE `replacement_request` (
  `RequestID` int(11) NOT NULL,
  `TransactionID` int(11) DEFAULT NULL,
  `Reason` text DEFAULT NULL,
  `DateRequested` timestamp NOT NULL DEFAULT current_timestamp(),
  `Status` enum('Pending','Approved','Rejected') DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `replacement_request`
--

INSERT INTO `replacement_request` (`RequestID`, `TransactionID`, `Reason`, `DateRequested`, `Status`) VALUES
(1, 1, 'Engine performance not as described.', '2024-11-24 16:07:47', 'Pending'),
(2, 2, 'Paint damage on delivery.', '2024-11-24 16:07:47', 'Approved'),
(3, 3, 'Battery issue.', '2024-11-24 16:07:47', 'Rejected'),
(4, 4, 'Tires are not in good condition.', '2024-11-24 16:07:47', 'Pending'),
(5, 5, 'Noisy engine.', '2024-11-24 16:07:47', 'Approved');

-- --------------------------------------------------------

--
-- Table structure for table `reported_issues`
--

CREATE TABLE `reported_issues` (
  `IssueID` int(11) NOT NULL,
  `BuyerID` int(11) DEFAULT NULL,
  `ProductID` int(11) DEFAULT NULL,
  `Description` text DEFAULT NULL,
  `DateReported` timestamp NOT NULL DEFAULT current_timestamp(),
  `Status` enum('Pending','Resolved','Dismissed') DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `reported_issues`
--

INSERT INTO `reported_issues` (`IssueID`, `BuyerID`, `ProductID`, `Description`, `DateReported`, `Status`) VALUES
(1, 1, 1, 'The car has unexpected engine noise.', '2024-11-24 16:04:18', 'Pending'),
(2, 2, 2, 'Minor scratches not mentioned in the listing.', '2024-11-24 16:04:18', 'Resolved'),
(3, 3, 3, 'Battery life is below average.', '2024-11-24 16:04:18', 'Pending'),
(4, 4, 4, 'Tires seem worn out.', '2024-11-24 16:04:18', 'Dismissed'),
(5, 5, 5, 'Air conditioning not functioning.', '2024-11-24 16:04:18', 'Pending');

-- --------------------------------------------------------

--
-- Table structure for table `reviews`
--

CREATE TABLE `reviews` (
  `ReviewID` int(11) NOT NULL,
  `ProductID` int(11) DEFAULT NULL,
  `BuyerID` int(11) DEFAULT NULL,
  `Rating` int(11) DEFAULT NULL CHECK (`Rating` between 1 and 5),
  `Comment` text DEFAULT NULL,
  `Date` timestamp NOT NULL DEFAULT current_timestamp()
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `reviews`
--

INSERT INTO `reviews` (`ReviewID`, `ProductID`, `BuyerID`, `Rating`, `Comment`, `Date`) VALUES
(1, 1, 1, 5, 'Excellent car, very satisfied!', '2024-11-24 16:02:39'),
(2, 2, 2, 4, 'Good condition, worth the price.', '2024-11-24 16:02:39'),
(3, 3, 3, 3, 'Average experience, minor issues.', '2024-11-24 16:02:39'),
(4, 4, 4, 5, 'Amazing value, highly recommend!', '2024-11-24 16:02:39'),
(5, 5, 5, 4, 'Overall good, a few scratches.', '2024-11-24 16:02:39');

-- --------------------------------------------------------

--
-- Table structure for table `sales_report`
--

CREATE TABLE `sales_report` (
  `ReportID` int(11) NOT NULL,
  `SellerID` int(11) DEFAULT NULL,
  `TotalSales` decimal(10,2) DEFAULT NULL,
  `NumberOfCarsSold` int(11) DEFAULT NULL,
  `ReportDate` date DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `sales_report`
--

INSERT INTO `sales_report` (`ReportID`, `SellerID`, `TotalSales`, `NumberOfCarsSold`, `ReportDate`) VALUES
(1, 1, 15000.00, 4, '2024-12-09'),
(2, 2, 12000.00, 3, '2024-12-09'),
(3, 3, 4000.00, 2, '2024-12-09'),
(4, 4, 5500.00, 2, '2024-12-09'),
(5, 5, 5200.00, 1, '2024-11-05');

-- --------------------------------------------------------

--
-- Table structure for table `seller`
--

CREATE TABLE `seller` (
  `SellerID` int(11) NOT NULL,
  `Name` varchar(100) DEFAULT NULL,
  `Email` varchar(100) DEFAULT NULL,
  `Phone` varchar(15) DEFAULT NULL,
  `Address` varchar(255) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `seller`
--

INSERT INTO `seller` (`SellerID`, `Name`, `Email`, `Phone`, `Address`) VALUES
(1, 'Frank Harris', 'frank@example.com', '1234509876', '101 Birch St'),
(2, 'Grace Lee', 'grace@example.com', '9876543210', '202 Walnut St'),
(3, 'Henry Adams', 'henry@example.com', '1029384756', '303 Cedar St'),
(4, 'Ivy Clark', 'ivy@example.com', '5647382910', '404 Chestnut St'),
(5, 'Jack White', 'jack@example.com', '6574839201', '505 Aspen St');

-- --------------------------------------------------------

--
-- Stand-in structure for view `sellerperformance`
-- (See below for the actual view)
--
CREATE TABLE `sellerperformance` (
`SellerID` int(11)
,`SellerName` varchar(100)
,`TotalSalesAmount` decimal(32,2)
,`TotalCarsSold` decimal(32,0)
,`LastReportDate` date
);

-- --------------------------------------------------------

--
-- Table structure for table `selling_history`
--

CREATE TABLE `selling_history` (
  `MergedID` int(11) NOT NULL,
  `BuyerName` varchar(100) DEFAULT NULL,
  `CarName` varchar(100) DEFAULT NULL,
  `CarPrice` decimal(10,2) DEFAULT NULL,
  `TotalAmount` decimal(10,2) DEFAULT NULL,
  `TransactionDate` date DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `selling_history`
--

INSERT INTO `selling_history` (`MergedID`, `BuyerName`, `CarName`, `CarPrice`, `TotalAmount`, `TransactionDate`) VALUES
(1, 'Alice Johnson', 'Toyota Corolla', 5000.00, 5000.00, '2024-11-24'),
(2, 'Bob Smith', 'Honda Civic', 6000.00, 6000.00, '2024-11-24'),
(3, 'Charlie Brown', 'Ford Focus', 4000.00, 4000.00, '2024-11-24'),
(4, 'Diana Ross', 'Chevrolet Malibu', 5500.00, 5500.00, '2024-11-24'),
(5, 'Eve Taylor', 'Nissan Altima', 5200.00, 5200.00, '2024-11-24'),
(6, 'Alice Johnson', 'Toyota Corolla', 5000.00, 5000.00, '2024-11-24'),
(7, 'Alice Johnson', 'Honda Civic', 6000.00, 4800.00, '2024-12-01'),
(8, 'Bob Smith', 'Honda Civic', 6000.00, 6000.00, '2024-11-24'),
(9, 'Bob Smith', 'Toyota Corolla', 5000.00, 5500.00, '2024-12-02'),
(10, 'Charlie Brown', 'Ford Focus', 4000.00, 4000.00, '2024-11-24'),
(11, 'Charlie Brown', 'Nissan Altima', 5200.00, 6200.00, '2024-12-03'),
(12, 'Diana Ross', 'Chevrolet Malibu', 3500.00, 5500.00, '2024-11-24'),
(13, 'Diana Ross', 'Ford Focus', 4000.00, 25000.00, '2024-12-04'),
(14, 'Eve Taylor', 'Nissan Altima', 5200.00, 5200.00, '2024-11-24'),
(15, 'Eve Taylor', 'Toyota Corolla', 5000.00, 35000.00, '2024-12-05'),
(21, 'Alice Johnson', 'Toyota Corolla', 5000.00, 5000.00, '2024-11-24'),
(22, 'Alice Johnson', 'Honda Civic', 6000.00, 4800.00, '2024-12-01'),
(23, 'Bob Smith', 'Honda Civic', 6000.00, 6000.00, '2024-11-24'),
(24, 'Bob Smith', 'Toyota Corolla', 5000.00, 5500.00, '2024-12-02'),
(25, 'Charlie Brown', 'Ford Focus', 4000.00, 4000.00, '2024-11-24'),
(26, 'Charlie Brown', 'Nissan Altima', 5200.00, 6200.00, '2024-12-03'),
(27, 'Diana Ross', 'Chevrolet Malibu', 3500.00, 5500.00, '2024-11-24'),
(28, 'Diana Ross', 'Ford Focus', 4000.00, 25000.00, '2024-12-04'),
(29, 'Eve Taylor', 'Nissan Altima', 5200.00, 5200.00, '2024-11-24'),
(30, 'Eve Taylor', 'Toyota Corolla', 5000.00, 35000.00, '2024-12-05');

-- --------------------------------------------------------

--
-- Table structure for table `sold_car`
--

CREATE TABLE `sold_car` (
  `SoldCarID` int(11) NOT NULL,
  `ProductID` int(11) DEFAULT NULL,
  `TransactionID` int(11) DEFAULT NULL,
  `Name` varchar(100) DEFAULT NULL,
  `Model` varchar(50) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `sold_car`
--

INSERT INTO `sold_car` (`SoldCarID`, `ProductID`, `TransactionID`, `Name`, `Model`) VALUES
(1, 1, 1, 'Toyota Corolla', '2010 model, excellent condition'),
(2, 4, 4, 'Chevrolet Malibu', '2013 model, fully serviced'),
(3, 1, 1, 'Toyota Corolla', '2010 model, excellent condition');

-- --------------------------------------------------------

--
-- Table structure for table `sponsored_service_providers`
--

CREATE TABLE `sponsored_service_providers` (
  `ProviderID` int(11) NOT NULL,
  `Name` varchar(100) DEFAULT NULL,
  `ContactInfo` varchar(255) DEFAULT NULL,
  `ServiceType` varchar(100) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `sponsored_service_providers`
--

INSERT INTO `sponsored_service_providers` (`ProviderID`, `Name`, `ContactInfo`, `ServiceType`) VALUES
(1, 'AutoFix Inc.', 'autofix@example.com, 123-456-7890', 'Car Repair'),
(2, 'TirePro', 'tirepro@example.com, 234-567-8901', 'Tire Replacement'),
(3, 'Shine & Go', 'shinego@example.com, 345-678-9012', 'Car Cleaning'),
(4, 'BatteryCare', 'batterycare@example.com, 456-789-0123', 'Battery Replacement'),
(5, 'Detail Masters', 'detailmasters@example.com, 567-890-1234', 'Car Detailing');

-- --------------------------------------------------------

--
-- Table structure for table `stuff_list`
--

CREATE TABLE `stuff_list` (
  `StaffID` int(11) NOT NULL,
  `Name` varchar(100) DEFAULT NULL,
  `Role` varchar(100) DEFAULT NULL,
  `ContactInfo` varchar(255) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `stuff_list`
--

INSERT INTO `stuff_list` (`StaffID`, `Name`, `Role`, `ContactInfo`) VALUES
(1, 'John Doe', 'Inspector', 'john@example.com, 123-456-7890'),
(2, 'Jane Smith', 'Customer Support', 'jane@example.com, 234-567-8901'),
(3, 'Alex Brown', 'Admin', 'alex@example.com, 345-678-9012'),
(4, 'Emily Davis', 'Sales', 'emily@example.com, 456-789-0123'),
(5, 'Chris Wilson', 'Maintenance', 'chris@example.com, 567-890-1234');

-- --------------------------------------------------------

--
-- Table structure for table `transaction`
--

CREATE TABLE `transaction` (
  `TransactionID` int(11) NOT NULL,
  `BuyerID` int(11) DEFAULT NULL,
  `ProductID` int(11) DEFAULT NULL,
  `Date` timestamp NOT NULL DEFAULT current_timestamp(),
  `Amount` decimal(10,2) DEFAULT NULL,
  `Status` enum('Pending','Completed','Cancelled') DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `transaction`
--

INSERT INTO `transaction` (`TransactionID`, `BuyerID`, `ProductID`, `Date`, `Amount`, `Status`) VALUES
(1, 1, 1, '2024-11-24 15:53:55', 5000.00, 'Completed'),
(2, 2, 2, '2024-11-24 15:53:55', 6000.00, 'Pending'),
(3, 3, 3, '2024-11-24 15:53:55', 4000.00, 'Cancelled'),
(4, 4, 4, '2024-11-24 15:53:55', 5500.00, 'Completed'),
(5, 5, 5, '2024-11-24 15:53:55', 5200.00, 'Pending'),
(11, 1, 2, '2024-11-30 18:00:00', 4800.00, 'Completed'),
(12, 2, 1, '2024-12-01 18:00:00', 5500.00, 'Completed'),
(13, 3, 5, '2024-12-02 18:00:00', 6200.00, 'Completed'),
(14, 4, 3, '2024-12-03 18:00:00', 25000.00, 'Completed'),
(15, 5, 1, '2024-12-04 18:00:00', 35000.00, 'Completed');

-- --------------------------------------------------------

--
-- Table structure for table `wishlist`
--

CREATE TABLE `wishlist` (
  `WishlistID` int(11) NOT NULL,
  `BuyerID` int(11) DEFAULT NULL,
  `ProductID` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `wishlist`
--

INSERT INTO `wishlist` (`WishlistID`, `BuyerID`, `ProductID`) VALUES
(1, 1, 2),
(2, 2, 3),
(3, 3, 4),
(4, 4, 5),
(5, 5, 1),
(6, 1, 3);

-- --------------------------------------------------------

--
-- Structure for view `buyerpurchasehistory`
--
DROP TABLE IF EXISTS `buyerpurchasehistory`;

CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL SECURITY DEFINER VIEW `buyerpurchasehistory`  AS SELECT `buyer`.`BuyerID` AS `BuyerID`, `buyer`.`Name` AS `BuyerName`, `product`.`Name` AS `CarName`, `product`.`Price` AS `CarPrice`, `transaction`.`Date` AS `TransactionDate`, `transaction`.`Status` AS `TransactionStatus` FROM ((`buyer` join `transaction` on(`buyer`.`BuyerID` = `transaction`.`BuyerID`)) join `product` on(`transaction`.`ProductID` = `product`.`ProductID`)) ;

-- --------------------------------------------------------

--
-- Structure for view `mostsearchedcars`
--
DROP TABLE IF EXISTS `mostsearchedcars`;

CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL SECURITY DEFINER VIEW `mostsearchedcars`  AS SELECT `most_searched_car`.`CarName` AS `CarName`, `most_searched_car`.`SearchCount` AS `SearchCount` FROM `most_searched_car` ORDER BY `most_searched_car`.`SearchCount` DESC ;

-- --------------------------------------------------------

--
-- Structure for view `sellerperformance`
--
DROP TABLE IF EXISTS `sellerperformance`;

CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL SECURITY DEFINER VIEW `sellerperformance`  AS SELECT `seller`.`SellerID` AS `SellerID`, `seller`.`Name` AS `SellerName`, sum(`sales_report`.`TotalSales`) AS `TotalSalesAmount`, sum(`sales_report`.`NumberOfCarsSold`) AS `TotalCarsSold`, max(`sales_report`.`ReportDate`) AS `LastReportDate` FROM (`seller` join `sales_report` on(`seller`.`SellerID` = `sales_report`.`SellerID`)) GROUP BY `seller`.`SellerID`, `seller`.`Name` ;

--
-- Indexes for dumped tables
--

--
-- Indexes for table `admin`
--
ALTER TABLE `admin`
  ADD PRIMARY KEY (`AdminID`),
  ADD UNIQUE KEY `Email` (`Email`);

--
-- Indexes for table `buyer`
--
ALTER TABLE `buyer`
  ADD PRIMARY KEY (`BuyerID`),
  ADD UNIQUE KEY `Email` (`Email`);

--
-- Indexes for table `car_paper_verification`
--
ALTER TABLE `car_paper_verification`
  ADD PRIMARY KEY (`VerificationID`),
  ADD KEY `ProductID` (`ProductID`);

--
-- Indexes for table `faq`
--
ALTER TABLE `faq`
  ADD PRIMARY KEY (`FAQID`);

--
-- Indexes for table `highest_priced_car`
--
ALTER TABLE `highest_priced_car`
  ADD PRIMARY KEY (`RecordID`),
  ADD KEY `ProductID` (`ProductID`);

--
-- Indexes for table `lowest_priced_car`
--
ALTER TABLE `lowest_priced_car`
  ADD PRIMARY KEY (`RecordID`),
  ADD KEY `ProductID` (`ProductID`);

--
-- Indexes for table `maintenance_cost`
--
ALTER TABLE `maintenance_cost`
  ADD PRIMARY KEY (`MaintenanceID`),
  ADD KEY `ProductID` (`ProductID`);

--
-- Indexes for table `most_searched_car`
--
ALTER TABLE `most_searched_car`
  ADD PRIMARY KEY (`RecordID`);

--
-- Indexes for table `out_of_stock_cars`
--
ALTER TABLE `out_of_stock_cars`
  ADD PRIMARY KEY (`RecordID`),
  ADD KEY `ProductID` (`ProductID`);

--
-- Indexes for table `product`
--
ALTER TABLE `product`
  ADD PRIMARY KEY (`ProductID`),
  ADD KEY `SellerID` (`SellerID`);

--
-- Indexes for table `refund_request`
--
ALTER TABLE `refund_request`
  ADD PRIMARY KEY (`RefundID`),
  ADD KEY `TransactionID` (`TransactionID`);

--
-- Indexes for table `replacement_request`
--
ALTER TABLE `replacement_request`
  ADD PRIMARY KEY (`RequestID`),
  ADD KEY `TransactionID` (`TransactionID`);

--
-- Indexes for table `reported_issues`
--
ALTER TABLE `reported_issues`
  ADD PRIMARY KEY (`IssueID`),
  ADD KEY `BuyerID` (`BuyerID`),
  ADD KEY `ProductID` (`ProductID`);

--
-- Indexes for table `reviews`
--
ALTER TABLE `reviews`
  ADD PRIMARY KEY (`ReviewID`),
  ADD KEY `ProductID` (`ProductID`),
  ADD KEY `BuyerID` (`BuyerID`);

--
-- Indexes for table `sales_report`
--
ALTER TABLE `sales_report`
  ADD PRIMARY KEY (`ReportID`),
  ADD KEY `SellerID` (`SellerID`);

--
-- Indexes for table `seller`
--
ALTER TABLE `seller`
  ADD PRIMARY KEY (`SellerID`),
  ADD UNIQUE KEY `Email` (`Email`);

--
-- Indexes for table `selling_history`
--
ALTER TABLE `selling_history`
  ADD PRIMARY KEY (`MergedID`);

--
-- Indexes for table `sold_car`
--
ALTER TABLE `sold_car`
  ADD PRIMARY KEY (`SoldCarID`),
  ADD KEY `ProductID` (`ProductID`),
  ADD KEY `TransactionID` (`TransactionID`);

--
-- Indexes for table `sponsored_service_providers`
--
ALTER TABLE `sponsored_service_providers`
  ADD PRIMARY KEY (`ProviderID`);

--
-- Indexes for table `stuff_list`
--
ALTER TABLE `stuff_list`
  ADD PRIMARY KEY (`StaffID`);

--
-- Indexes for table `transaction`
--
ALTER TABLE `transaction`
  ADD PRIMARY KEY (`TransactionID`),
  ADD KEY `BuyerID` (`BuyerID`),
  ADD KEY `ProductID` (`ProductID`);

--
-- Indexes for table `wishlist`
--
ALTER TABLE `wishlist`
  ADD PRIMARY KEY (`WishlistID`),
  ADD KEY `BuyerID` (`BuyerID`),
  ADD KEY `ProductID` (`ProductID`);

--
-- AUTO_INCREMENT for dumped tables
--

--
-- AUTO_INCREMENT for table `admin`
--
ALTER TABLE `admin`
  MODIFY `AdminID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `buyer`
--
ALTER TABLE `buyer`
  MODIFY `BuyerID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `car_paper_verification`
--
ALTER TABLE `car_paper_verification`
  MODIFY `VerificationID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=15;

--
-- AUTO_INCREMENT for table `faq`
--
ALTER TABLE `faq`
  MODIFY `FAQID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `highest_priced_car`
--
ALTER TABLE `highest_priced_car`
  MODIFY `RecordID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `lowest_priced_car`
--
ALTER TABLE `lowest_priced_car`
  MODIFY `RecordID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `maintenance_cost`
--
ALTER TABLE `maintenance_cost`
  MODIFY `MaintenanceID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `most_searched_car`
--
ALTER TABLE `most_searched_car`
  MODIFY `RecordID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `out_of_stock_cars`
--
ALTER TABLE `out_of_stock_cars`
  MODIFY `RecordID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `product`
--
ALTER TABLE `product`
  MODIFY `ProductID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=55;

--
-- AUTO_INCREMENT for table `refund_request`
--
ALTER TABLE `refund_request`
  MODIFY `RefundID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `replacement_request`
--
ALTER TABLE `replacement_request`
  MODIFY `RequestID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `reported_issues`
--
ALTER TABLE `reported_issues`
  MODIFY `IssueID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `reviews`
--
ALTER TABLE `reviews`
  MODIFY `ReviewID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `sales_report`
--
ALTER TABLE `sales_report`
  MODIFY `ReportID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=14;

--
-- AUTO_INCREMENT for table `seller`
--
ALTER TABLE `seller`
  MODIFY `SellerID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `selling_history`
--
ALTER TABLE `selling_history`
  MODIFY `MergedID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=31;

--
-- AUTO_INCREMENT for table `sold_car`
--
ALTER TABLE `sold_car`
  MODIFY `SoldCarID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=4;

--
-- AUTO_INCREMENT for table `sponsored_service_providers`
--
ALTER TABLE `sponsored_service_providers`
  MODIFY `ProviderID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `stuff_list`
--
ALTER TABLE `stuff_list`
  MODIFY `StaffID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `transaction`
--
ALTER TABLE `transaction`
  MODIFY `TransactionID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=16;

--
-- AUTO_INCREMENT for table `wishlist`
--
ALTER TABLE `wishlist`
  MODIFY `WishlistID` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=7;

--
-- Constraints for dumped tables
--

--
-- Constraints for table `car_paper_verification`
--
ALTER TABLE `car_paper_verification`
  ADD CONSTRAINT `car_paper_verification_ibfk_1` FOREIGN KEY (`ProductID`) REFERENCES `product` (`ProductID`);

--
-- Constraints for table `highest_priced_car`
--
ALTER TABLE `highest_priced_car`
  ADD CONSTRAINT `highest_priced_car_ibfk_1` FOREIGN KEY (`ProductID`) REFERENCES `product` (`ProductID`);

--
-- Constraints for table `lowest_priced_car`
--
ALTER TABLE `lowest_priced_car`
  ADD CONSTRAINT `lowest_priced_car_ibfk_1` FOREIGN KEY (`ProductID`) REFERENCES `product` (`ProductID`);

--
-- Constraints for table `maintenance_cost`
--
ALTER TABLE `maintenance_cost`
  ADD CONSTRAINT `maintenance_cost_ibfk_1` FOREIGN KEY (`ProductID`) REFERENCES `product` (`ProductID`);

--
-- Constraints for table `out_of_stock_cars`
--
ALTER TABLE `out_of_stock_cars`
  ADD CONSTRAINT `out_of_stock_cars_ibfk_1` FOREIGN KEY (`ProductID`) REFERENCES `product` (`ProductID`);

--
-- Constraints for table `product`
--
ALTER TABLE `product`
  ADD CONSTRAINT `product_ibfk_1` FOREIGN KEY (`SellerID`) REFERENCES `seller` (`SellerID`);

--
-- Constraints for table `refund_request`
--
ALTER TABLE `refund_request`
  ADD CONSTRAINT `refund_request_ibfk_1` FOREIGN KEY (`TransactionID`) REFERENCES `transaction` (`TransactionID`);

--
-- Constraints for table `replacement_request`
--
ALTER TABLE `replacement_request`
  ADD CONSTRAINT `replacement_request_ibfk_1` FOREIGN KEY (`TransactionID`) REFERENCES `transaction` (`TransactionID`);

--
-- Constraints for table `reported_issues`
--
ALTER TABLE `reported_issues`
  ADD CONSTRAINT `reported_issues_ibfk_1` FOREIGN KEY (`BuyerID`) REFERENCES `buyer` (`BuyerID`),
  ADD CONSTRAINT `reported_issues_ibfk_2` FOREIGN KEY (`ProductID`) REFERENCES `product` (`ProductID`);

--
-- Constraints for table `reviews`
--
ALTER TABLE `reviews`
  ADD CONSTRAINT `reviews_ibfk_1` FOREIGN KEY (`ProductID`) REFERENCES `product` (`ProductID`),
  ADD CONSTRAINT `reviews_ibfk_2` FOREIGN KEY (`BuyerID`) REFERENCES `buyer` (`BuyerID`);

--
-- Constraints for table `sales_report`
--
ALTER TABLE `sales_report`
  ADD CONSTRAINT `sales_report_ibfk_1` FOREIGN KEY (`SellerID`) REFERENCES `seller` (`SellerID`);

--
-- Constraints for table `sold_car`
--
ALTER TABLE `sold_car`
  ADD CONSTRAINT `sold_car_ibfk_1` FOREIGN KEY (`ProductID`) REFERENCES `product` (`ProductID`),
  ADD CONSTRAINT `sold_car_ibfk_2` FOREIGN KEY (`TransactionID`) REFERENCES `transaction` (`TransactionID`);

--
-- Constraints for table `transaction`
--
ALTER TABLE `transaction`
  ADD CONSTRAINT `transaction_ibfk_1` FOREIGN KEY (`BuyerID`) REFERENCES `buyer` (`BuyerID`),
  ADD CONSTRAINT `transaction_ibfk_2` FOREIGN KEY (`ProductID`) REFERENCES `product` (`ProductID`);

--
-- Constraints for table `wishlist`
--
ALTER TABLE `wishlist`
  ADD CONSTRAINT `wishlist_ibfk_1` FOREIGN KEY (`BuyerID`) REFERENCES `buyer` (`BuyerID`),
  ADD CONSTRAINT `wishlist_ibfk_2` FOREIGN KEY (`ProductID`) REFERENCES `product` (`ProductID`);
COMMIT;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
