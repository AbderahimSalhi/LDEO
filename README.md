# LDEO
# MicroAeath CSV Data Visualization Script
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import seaborn as sns
from datetime import datetime

sns.set()
sns.set_context("poster")

data = pd.read_csv('MA350-0014_S0130_190805125324.csv')
data['Time local (hh:mm:ss)']= pd.to_datetime(data['Time local (hh:mm:ss)']).dt.time

fig, ax = plt.subplots(5,1, figsize=(28,45))
#fig.suptitle("Cigarette Smoking Machine Experiment with BC: 8/5/2019 \n MA350-0014_S0130", size=30)

data['UV BC1'] = data['UV BC1'] / 1000 # Reduce the concentration unit to save space on the plot
data['IR BC1'] = data['IR BC1'] / 1000 
data['UV BCc'] = data['UV BCc'] / 1000
data['IR BCc'] = data['IR BCc'] / 1000
data['$\Delta UVPM1$'] = data['UV BC1'] - data['IR BC1'] # Create a new column 'ΔUVPM1'to calculate (UV BC1 - IR BC1)
data['$\Delta UVPMc$'] = data['UV BCc'] - data['IR BCc'] # Create a new column 'ΔUVPMc'to calculate (UV Bcc - IR BCc)

plot1 = plt.subplot(511) # 5 rows, 1 columns, plot 1
plt.plot(data['Time local (hh:mm:ss)'], data['UV BC1'], Label='UV BC1',color='aqua') 
plt.plot(data['Time local (hh:mm:ss)'], data['UV BC1'].rolling(12).mean(), label= 'UV BC1 Moving Avg/5min',color='b')
plt.plot(data['Time local (hh:mm:ss)'], data['IR BC1'], label= 'IR BC1', color='deeppink')
plt.plot(data['Time local (hh:mm:ss)'], data['IR BC1'].rolling(12).mean(), label= 'IR BC1 Moving Avg/5min', color='lime')
plt.legend(loc='best', fontsize=20)
plt.xlabel('Time', size=30)
plt.ylabel('UV & IR BC1 (µg/$m^3$)', size=35)
#plt.grid(True, color='k', linestyle=':', linewidth=2)

plot2 = plt.subplot(512) # 5 rows, 1 columns, plot 2
plt.plot(data['Time local (hh:mm:ss)'], data['UV ATN1'], color='y')
plt.plot(data['Time local (hh:mm:ss)'], data['IR ATN1'], color='r')
plt.legend(loc='best', fontsize=20)
plt.xlabel('Time', size=30)
plt.ylabel('Attenuation', size=40)
#plt.grid(True)

plot3 = plt.subplot(513) # 5 rows, 1 columns, plot 3
plt.plot(data['Time local (hh:mm:ss)'], data['UV BCc'], label= 'UV BCc', color='lime')
plt.plot(data['Time local (hh:mm:ss)'], data['UV BCc'].rolling(12).mean(), label= 'UV BCc Moving Avg/1min', color='k')
plt.plot(data['Time local (hh:mm:ss)'], data['IR BCc'].rolling(30).mean(), label= 'IR BCc Moving Avg/1min', color='c')
plt.plot(data['Time local (hh:mm:ss)'], data['IR BCc'].rolling(12).mean(), label= 'IR BCc Moving Avg/1min', color='r')
plt.legend(loc='best', fontsize=20)
plt.xlabel('Time', size=30)
plt.ylabel('UV & IR BCc (µg/$m^3$)', size=35)
#plt.grid(True)

plot4 = plt.subplot(514) # 5 rows, 1 columns, plot 4
plt.plot(data['Time local (hh:mm:ss)'], data['$\Delta UVPM1$'].rolling(12).mean(),label= '$\Delta UVPM1$ Moving Avg/5min', color='b')
plt.plot(data['Time local (hh:mm:ss)'], data['$\Delta UVPMc$'].rolling(12).mean(),label= '$\Delta UVPMc$ Moving Avg/5min', color='r')
plt.legend(loc='best', fontsize=20)
plt.xlabel('Time', size=30)
plt.ylabel('$\Delta UVPM1$ vs $\Delta UVPMc$ (µg/$m^3$)', size=35)
#plt.grid(True)

plot5 = plt.subplot(515) # 5 rows, 1 columns, plot 5(lambda s: s[:-3], data['Time local (hh:mm:ss)'] )
plt.plot(data['Time local (hh:mm:ss)'], data['Flow1 (mL/min)'], color='b')
plt.plot(data['Time local (hh:mm:ss)'], data['Flow2 (mL/min)'], color='r')
plt.legend(loc='best', fontsize=22)
plt.ylabel('Flow Rate (mL/min)', size=35)
plt.xlabel('Time', size=30)
plt.grid(True)

plt.subplots_adjust(top=.98, bottom=0.05, left=0.10, right=.98, hspace=0.15, wspace=.15)
#plt.rc('xtick',labelsize=32)
#plt.rc('ytick',labelsize=32)

plt.show()

fig.savefig('MA350-0014.jpeg')


