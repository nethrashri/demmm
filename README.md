import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the data
df = pd.read_csv('user_device_usage_pattern_V4.csv')

# Basic statistics
print("Basic Statistics:")
print(df[['signal_strength', 'packet_loss_rate', 'latency', 'jitter_ms', 'bandwidth_speed_per_sec_mbps']].describe())

# Histograms for each network metric
plt.figure(figsize=(15, 10))
for i, column in enumerate(['signal_strength', 'packet_loss_rate', 'latency', 'jitter_ms', 'bandwidth_speed_per_sec_mbps'], 1):
    plt.subplot(2, 3, i)
    sns.histplot(df[column], kde=True)
    plt.title(f'Distribution of {column}')
plt.tight_layout()
plt.show()

# Box plots to check for outliers
plt.figure(figsize=(15, 10))
for i, column in enumerate(['signal_strength', 'packet_loss_rate', 'latency', 'jitter_ms', 'bandwidth_speed_per_sec_mbps'], 1):
    plt.subplot(2, 3, i)
    sns.boxplot(x=df[column])
    plt.title(f'Box plot of {column}')
plt.tight_layout()
plt.show()

# Correlation matrix
correlation = df[['signal_strength', 'packet_loss_rate', 'latency', 'jitter_ms', 'bandwidth_speed_per_sec_mbps']].corr()
plt.figure(figsize=(8, 6))
sns.heatmap(correlation, annot=True, cmap='coolwarm', fmt=".2f")
plt.title('Correlation Matrix')
plt.show()

# Time series analysis of average metrics by hour (assuming timestamp is in correct format)
df['timestamp'] = pd.to_datetime(df['timestamp'])
df.set_index('timestamp', inplace=True)
plt.figure(figsize=(15, 10))
for i, column in enumerate(['signal_strength', 'packet_loss_rate', 'latency', 'jitter_ms', 'bandwidth_speed_per_sec_mbps'], 1):
    plt.subplot(2, 3, i)
    df[column].resample('H').mean().plot()
    plt.title(f'Hourly Trend of {column}')
    plt.ylabel(column)
plt.tight_layout()
plt.show()
