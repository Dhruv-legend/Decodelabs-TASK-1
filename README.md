# Decodelabs-TASK-1

import pandas as pd

# Load the excel file to see its sheets and structure
file_path = "Dataset for Data Analytics.xlsx"
try:
    xls = pd.ExcelFile(file_path)
    print("Sheet names:", xls.sheet_names)
    
    # Read the first sheet
    df = pd.read_excel(file_path, sheet_name=xls.sheet_names[0])
    
    print("\n--- DataFrame Info ---")
    df.info()
    
    print("\n--- First 5 Rows ---")
    print(df.head())
    
    print("\n--- Missing Values ---")
    print(df.isnull().sum())
    
    print("\n--- Duplicates ---")
    print("Total duplicate rows:", df.duplicated().sum())
    
except Exception as e:
    print("Error:", e)


    import pandas as pd

file_path = "Dataset for Data Analytics.xlsx"
df = pd.read_excel(file_path)

print("Unique Products:", df['Product'].unique())
print("Unique Payment Methods:", df['PaymentMethod'].unique())
print("Unique Order Status:", df['OrderStatus'].unique())
print("Date Range:", df['Date'].min(), "to", df['Date'].min()) # Oops meant max
print("Negative quantities?", (df['Quantity'] < 0).sum())
print("Negative prices?", (df['UnitPrice'] < 0).sum())
print("TotalPrice Check (Quantity * UnitPrice - TotalPrice):")
diff = (df['Quantity'] * df['UnitPrice'] - df['TotalPrice']).abs().max()
print("Max diff:", diff)

# Let's fix the print
print("Max Date:", df['Date'].max())


print("Trailing/Leading spaces check:")
for col in df.select_dtypes(include=['object']).columns:
    has_space = df[col].str.startswith(' ') | df[col].str.endswith(' ')
    print(f"{col}: {has_space.sum()} rows with spaces")
    
print("Checking for coupon code values:")
print(df['CouponCode'].unique())

import pandas as pd

# Load data
file_path = "Dataset for Data Analytics.xlsx"
df = pd.read_excel(file_path)

initial_rows = len(df)
initial_nulls = df.isnull().sum().to_dict()

# 1. Identify and Handle Missing Values
df['CouponCode'] = df['CouponCode'].fillna('NO_COUPON')
final_nulls = df.isnull().sum().to_dict()

# 2. Remove Duplicates
df = df.drop_duplicates()
final_rows = len(df)
duplicates_removed = initial_rows - final_rows

# 3. Correct Data Formats
# Dates
df['Date'] = pd.to_datetime(df['Date']).dt.date

# Numbers
df['UnitPrice'] = df['UnitPrice'].round(2)
df['TotalPrice'] = df['TotalPrice'].round(2)

# Text (strip whitespaces and title case for categorical text)
text_cols = ['Product', 'PaymentMethod', 'OrderStatus', 'ReferralSource', 'ShippingAddress']
for col in text_cols:
    df[col] = df[col].astype(str).str.strip().str.title()
    
# CouponCode and IDs: uppercase and strip
id_cols = ['OrderID', 'CustomerID', 'TrackingNumber', 'CouponCode']
for col in id_cols:
    df[col] = df[col].astype(str).str.strip().str.upper()

# Save cleaned data
output_path = "Cleaned_Dataset_for_Data_Analytics.xlsx"
df.to_excel(output_path, index=False)

print(f"Cleaned dataset saved to {output_path}")
print(f"Duplicates removed: {duplicates_removed}")
print(f"Missing values filled: {initial_nulls['CouponCode']}")
