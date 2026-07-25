# Quantitative Research Virtual Experience Program on Forage

JP Morgan Chase & Co Forage virtual work experience tasks:

Task 1 - investigate and analyse price data via "Nat_Gas.xlsx"

Task 2 - price a commodity storage contract via "Nat_Gas.xlsx"

Task 3 - credit risk analysis via "task 3 and task 4_Loan_Data.csv"

Task 4 - bucket fico scores via "task 3 and task 4_Loan_Data.csv"

**"Used Jupyter on VSCode"**

**Step-by-Step Implementation Guide:**

Step 1: Install the Necessary Extensions
To run Jupyter Notebooks inside VS Code, you need to add a couple of extensions.
1. Open VS Code.
2. Click on the Extensions icon on the left sidebar.
3. Search for Python and click Install.
4. Search for Jupyter and click Install.

Step 2: Set Up Your Project Folder
Keep your files organized so VS Code can easily find your data.
1. Create a new folder on your computer.
2. Move the file you downloaded from Forage into this new folder.
3. In VS Code, go to File > Open Folder and select the folder you just created. You should now see the file in the Explorer pane on the left.

Step 3: Install Required Python Libraries
You need to install the data science packages used in the code.
1. Open a terminal inside VS Code by going to the top menu: Terminal > New Terminal.
2. In the terminal window at the bottom of the screen, type the following command and press Enter:

```ruby
pip install pandas numpy matplotlib scipy
```

Step 4: Create Your Jupyter Notebook
1. In the Explorer pane on the left, click the New File icon next to your folder name.
2. Name the file "solution.ipynb" and The ".ipynb" extension tells VS Code this is a Jupyter Notebook.
3. Open the file and You will see an empty cell where you can type code.

**Task 1 - investigate and analyse price data**

Step 1: Write and Run Your Code Cells

In a Jupyter Notebook, you run code in separate blocks (cells). Copy and paste the following code blocks into individual cells. To run a cell and see the output, click the Play button on the left of the cell or press Shift + Enter.

**Cell 1: Imports**
Copy this into the first cell and run it:
```ruby
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from datetime import timedelta
from scipy.optimize import curve_fit
```

**Cell 2: Load and Prepare Data**
Click "+ Code" to add a new cell, paste this, and run it:
```ruby
# Load the CSV file
df = pd.read_csv("Nat_Gas.csv")

# Convert dates using 'mixed' format to safely handle any year-length differences
df['Dates'] = pd.to_datetime(df['Dates'], format='mixed')
df = df.sort_values('Dates')

# Calculate days since the start of the dataset
min_date = df['Dates'].min()
df['Days'] = (df['Dates'] - min_date).dt.days

# Preview the first few rows to confirm it worked
df.head()
```

**Cell 3: Build and Fit the Model**
Add a new cell for the math model:
```ruby
def price_model(t, amplitude, frequency, phase_shift, slope, intercept):
    return amplitude * np.sin(frequency * t + phase_shift) + slope * t + intercept

# Initial guesses for the curve_fit algorithm
initial_guesses = [0.5, 2 * np.pi / 365.25, 0, 0.001, 10]

# Fit the model
popt, pcov = curve_fit(price_model, df['Days'], df['Prices'], p0=initial_guesses)
print("Optimal parameters found:", popt)
```

**Cell 4: The Estimation Function**
Add a new cell for the function Alex requested:
```ruby
def estimate_price(input_date_str):
    target_date = pd.to_datetime(input_date_str)
    days_since = (target_date - min_date).days
    estimated_price = price_model(days_since, *popt)
    return estimated_price

print(f"Estimated Price for Jan 15, 2022: ${estimate_price('2022-01-15'):.2f}")
print(f"Estimated Price for July 4, 2025: ${estimate_price('2025-07-04'):.2f}")
```

**Cell 5: Visualize the Data**
Add a final cell to generate the chart. This proves your model works visually:
```ruby
# Create future dates for extrapolation
future_days = np.arange(0, df['Days'].max() + 365, 1)
future_dates = [min_date + timedelta(days=int(d)) for d in future_days]
predicted_prices = price_model(future_days, *popt)

# Plot the data
plt.figure(figsize=(10, 6))
plt.plot(df['Dates'], df['Prices'], 'o', label='Historical Data')
plt.plot(future_dates, predicted_prices, '-', label='Model Fit & 1-Year Extrapolation')
plt.xlabel('Date')
plt.ylabel('Natural Gas Price ($)')
plt.title('Natural Gas Price Interpolation and Extrapolation')
plt.legend()
plt.grid(True)
plt.show()
```
Step 2: Save and Submit to Forage




```ruby
practice makes perfect
```
