-- Cleaning data in SQL Queries --

SELECT *
FROM NashvilleHousing

----------------------------------------------------------------------------------------------------------------------------

-- Standardize Date Format

SELECT SaleDate, CONVERT(Date,SaleDate)
FROM NashvilleHousing

ALTER TABLE NashvilleHousing
ADD SaleDateConverted Date

UPDATE NashvilleHousing
SET SaleDateConverted = CONVERT(Date,SaleDate)

SELECT SaleDate, SaleDateConverted
FROM NashvilleHousing

----------------------------------------------------------------------------------------------------------------------------

-- Populate Property Address data

SELECT *
FROM NashvilleHousing

SELECT PropertyAddress
FROM NashvilleHousing
WHERE PropertyAddress IS NULL

SELECT *
FROM NashvilleHousing
ORDER BY ParcelID

SELECT a.ParcelID, a.PropertyAddress, b.ParcelID, b.PropertyAddress, ISNULL(a.PropertyAddress, b.PropertyAddress)
FROM NashvilleHousing a
JOIN NashvilleHousing b
ON a.ParcelID = b.ParcelID
AND a.[UniqueID ]<>b.[UniqueID ]
WHERE a.PropertyAddress IS NULL

UPDATE a
SET PropertyAddress = ISNULL(a.PropertyAddress, b.PropertyAddress)
FROM NashvilleHousing a
JOIN NashvilleHousing b
ON a.ParcelID = b.ParcelID
AND a.[UniqueID ]<>b.[UniqueID ]
WHERE a.PropertyAddress IS NULL

----------------------------------------------------------------------------------------------------------------------------

-- Spliting Address into Seperated Columns (Addess, City, State)

SELECT PropertyAddress
FROM NashvilleHousing

SELECT SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress)-1) Address,
LEFT(PropertyAddress, CHARINDEX(',', PropertyAddress)-1),
RIGHT(PropertyAddress, (LEN(PropertyAddress) - CHARINDEX(',', PropertyAddress)))
FROM NashvilleHousing

ALTER TABLE NashvilleHousing
ADD PropertySplitAddress Nvarchar(255)

UPDATE NashvilleHousing
SET PropertySplitAddress = LEFT(PropertyAddress, CHARINDEX(',', PropertyAddress)-1)

ALTER TABLE NashvilleHousing
ADD PropertySplitCity Nvarchar(255)

UPDATE NashvilleHousing
SET PropertySplitCity = RIGHT(PropertyAddress, (LEN(PropertyAddress) - CHARINDEX(',', PropertyAddress)))

SELECT *
FROM NashvilleHousing

----------------------------------------------------------------------------------------------------------------------------

-- Spliting Owner Address into Seperated Columns (Addess, City, State) in different way.

SELECT OwnerAddress
FROM NashvilleHousing

SELECT PARSENAME(REPLACE(OwnerAddress,',','.'), 3),
PARSENAME(REPLACE(OwnerAddress,',','.'), 2),
PARSENAME(REPLACE(OwnerAddress,',','.'), 1)
FROM NashvilleHousing

ALTER TABLE NashvilleHousing
ADD OwnerSplitAddress Nvarchar(255)

UPDATE NashvilleHousing
SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress,',','.'), 3)

ALTER TABLE NashvilleHousing
ADD OwnerSplitCity Nvarchar(255)

UPDATE NashvilleHousing
SET OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress,',','.'), 2)

ALTER TABLE NashvilleHousing
ADD OwnerSplitState Nvarchar(255)

UPDATE NashvilleHousing
SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress,',','.'), 1)

SELECT *
FROM NashvilleHousing

----------------------------------------------------------------------------------------------------------------------------

-- Change 'Y' and 'N' to 'Yes' and 'No' in "Sold As Vacant" field

SELECT DISTINCT(SoldAsVacant), COUNT(SoldAsVacant)
FROM NashvilleHousing
GROUP BY SoldAsVacant
ORDER BY 2

SELECT SoldAsVacant,
CASE WHEN SoldAsVacant = 'Y' THEN 'Yes'
WHEN SoldAsVacant = 'N' THEN 'No'
ELSE SoldAsVacant
END
FROM NashvilleHousing

UPDATE NashvilleHousing
SET SoldAsVacant = CASE WHEN SoldAsVacant = 'Y' THEN 'Yes'
WHEN SoldAsVacant = 'N' THEN 'No'
ELSE SoldAsVacant
END

----------------------------------------------------------------------------------------------------------------------------

-- Remove Duplicated rows

WITH t1 AS(
SELECT *, ROW_NUMBER() OVER (PARTITION BY ParcelID, 
					  PropertyAddress, 
					  SalePrice, 
					  SaleDate,
					  LegalReference
					  ORDER BY UniqueID) as rownum
FROM NashvilleHousing
)
SELECT *
FROM t1
WHERE rownum > 1

WITH t1 AS(
SELECT *, ROW_NUMBER() OVER (PARTITION BY ParcelID, 
					  PropertyAddress, 
					  SalePrice, 
					  SaleDate,
					  LegalReference
					  ORDER BY UniqueID) as rownum
FROM NashvilleHousing
)
DELETE
FROM t1
WHERE rownum > 1

----------------------------------------------------------------------------------------------------------------------------

-- Delete Unused Columns

ALTER TABLE NashvilleHousing
DROP COLUMN OwnerAddress, TaxDistrict, PropertyAddress

SELECT *
FROM NashvilleHousing

ALTER TABLE NashvilleHousing
DROP COLUMN SaleDate
