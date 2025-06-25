{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Cyber Security Analytics\n",
    "\n",
    "Conducting an investigation on a web server logs to identify malicious attack activity using Python data science libraries."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>date</th>\n",
       "      <th>time</th>\n",
       "      <th>s-ip</th>\n",
       "      <th>cs-method</th>\n",
       "      <th>cs-uri-stem</th>\n",
       "      <th>cs-uri-query</th>\n",
       "      <th>s-port</th>\n",
       "      <th>cs-username</th>\n",
       "      <th>c-ip</th>\n",
       "      <th>cs(User-Agent)</th>\n",
       "      <th>cs(Referer)</th>\n",
       "      <th>sc-status</th>\n",
       "      <th>sc-substatus</th>\n",
       "      <th>sc-win32-status</th>\n",
       "      <th>time-taken</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>2022-01-01</td>\n",
       "      <td>04:07:00</td>\n",
       "      <td>57.213.70.113</td>\n",
       "      <td>GET</td>\n",
       "      <td>template.css</td>\n",
       "      <td>v=uwixfkca</td>\n",
       "      <td>443</td>\n",
       "      <td>-</td>\n",
       "      <td>92.123.193.245</td>\n",
       "      <td>Mozilla/4.0+(compatible;+MSIE+6.0;+Windows+NT+...</td>\n",
       "      <td>-</td>\n",
       "      <td>200</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>24</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>2022-01-01</td>\n",
       "      <td>04:07:00</td>\n",
       "      <td>57.213.70.113</td>\n",
       "      <td>GET</td>\n",
       "      <td>template.css</td>\n",
       "      <td>v=igohhmua</td>\n",
       "      <td>443</td>\n",
       "      <td>-</td>\n",
       "      <td>92.123.193.245</td>\n",
       "      <td>Mozilla/4.0+(compatible;+MSIE+6.0;+Windows+NT+...</td>\n",
       "      <td>-</td>\n",
       "      <td>200</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>21</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>2022-01-01</td>\n",
       "      <td>04:07:00</td>\n",
       "      <td>57.213.70.113</td>\n",
       "      <td>GET</td>\n",
       "      <td>hsvlyggz.css</td>\n",
       "      <td>-</td>\n",
       "      <td>443</td>\n",
       "      <td>-</td>\n",
       "      <td>92.123.193.245</td>\n",
       "      <td>Mozilla/4.0+(compatible;+MSIE+6.0;+Windows+NT+...</td>\n",
       "      <td>-</td>\n",
       "      <td>404</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>26</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>2022-01-01</td>\n",
       "      <td>04:07:00</td>\n",
       "      <td>57.213.70.113</td>\n",
       "      <td>GET</td>\n",
       "      <td>index.aspx</td>\n",
       "      <td>-</td>\n",
       "      <td>443</td>\n",
       "      <td>-</td>\n",
       "      <td>92.123.193.245</td>\n",
       "      <td>Mozilla/4.0+(compatible;+MSIE+6.0;+Windows+NT+...</td>\n",
       "      <td>-</td>\n",
       "      <td>200</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>26</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>2022-01-01</td>\n",
       "      <td>04:07:16</td>\n",
       "      <td>57.213.70.113</td>\n",
       "      <td>GET</td>\n",
       "      <td>favico.ico</td>\n",
       "      <td>-</td>\n",
       "      <td>443</td>\n",
       "      <td>-</td>\n",
       "      <td>92.123.193.245</td>\n",
       "      <td>Mozilla/4.0+(compatible;+MSIE+6.0;+Windows+NT+...</td>\n",
       "      <td>https://bankofpunk.local/index.aspx</td>\n",
       "      <td>200</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>24</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>70705</th>\n",
       "      <td>2022-01-30</td>\n",
       "      <td>23:36:34</td>\n",
       "      <td>57.213.70.113</td>\n",
       "      <td>GET</td>\n",
       "      <td>footer.css</td>\n",
       "      <td>-</td>\n",
       "      <td>443</td>\n",
       "      <td>ry814066</td>\n",
       "      <td>194.202.68.159</td>\n",
       "      <td>Mozilla/5.0+(iPhone;+CPU+iPhone+OS+12_4_1+like...</td>\n",
       "      <td>https://bankofpunk.local/transactions.aspx</td>\n",
       "      <td>200</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>28</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>70706</th>\n",
       "      <td>2022-01-30</td>\n",
       "      <td>23:36:34</td>\n",
       "      <td>57.213.70.113</td>\n",
       "      <td>GET</td>\n",
       "      <td>ljjsfyon.js</td>\n",
       "      <td>v=599940</td>\n",
       "      <td>443</td>\n",
       "      <td>ry814066</td>\n",
       "      <td>194.202.68.159</td>\n",
       "      <td>Mozilla/5.0+(iPhone;+CPU+iPhone+OS+12_4_1+like...</td>\n",
       "      <td>https://bankofpunk.local/transactions.aspx</td>\n",
       "      <td>200</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>30</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>70707</th>\n",
       "      <td>2022-01-30</td>\n",
       "      <td>23:36:34</td>\n",
       "      <td>57.213.70.113</td>\n",
       "      <td>GET</td>\n",
       "      <td>transactions.aspx</td>\n",
       "      <td>page=3</td>\n",
       "      <td>443</td>\n",
       "      <td>ry814066</td>\n",
       "      <td>194.202.68.159</td>\n",
       "      <td>Mozilla/5.0+(iPhone;+CPU+iPhone+OS+12_4_1+like...</td>\n",
       "      <td>https://bankofpunk.local/transactions.aspx</td>\n",
       "      <td>200</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>22</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>70708</th>\n",
       "      <td>2022-01-30</td>\n",
       "      <td>23:36:46</td>\n",
       "      <td>57.213.70.113</td>\n",
       "      <td>GET</td>\n",
       "      <td>template.css</td>\n",
       "      <td>v=tvjgabsg</td>\n",
       "      <td>443</td>\n",
       "      <td>ry814066</td>\n",
       "      <td>194.202.68.159</td>\n",
       "      <td>Mozilla/5.0+(iPhone;+CPU+iPhone+OS+12_4_1+like...</td>\n",
       "      <td>https://bankofpunk.local/transactions.aspx</td>\n",
       "      <td>200</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>20</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>70709</th>\n",
       "      <td>2022-01-30</td>\n",
       "      <td>23:36:46</td>\n",
       "      <td>57.213.70.113</td>\n",
       "      <td>GET</td>\n",
       "      <td>transactions.aspx</td>\n",
       "      <td>page=4</td>\n",
       "      <td>443</td>\n",
       "      <td>ry814066</td>\n",
       "      <td>194.202.68.159</td>\n",
       "      <td>Mozilla/5.0+(iPhone;+CPU+iPhone+OS+12_4_1+like...</td>\n",
       "      <td>https://bankofpunk.local/transactions.aspx</td>\n",
       "      <td>200</td>\n",
       "      <td>0</td>\n",
       "      <td>0</td>\n",
       "      <td>25</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>70710 rows × 15 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "             date      time           s-ip cs-method        cs-uri-stem  \\\n",
       "0      2022-01-01  04:07:00  57.213.70.113       GET       template.css   \n",
       "1      2022-01-01  04:07:00  57.213.70.113       GET       template.css   \n",
       "2      2022-01-01  04:07:00  57.213.70.113       GET       hsvlyggz.css   \n",
       "3      2022-01-01  04:07:00  57.213.70.113       GET         index.aspx   \n",
       "4      2022-01-01  04:07:16  57.213.70.113       GET         favico.ico   \n",
       "...           ...       ...            ...       ...                ...   \n",
       "70705  2022-01-30  23:36:34  57.213.70.113       GET         footer.css   \n",
       "70706  2022-01-30  23:36:34  57.213.70.113       GET        ljjsfyon.js   \n",
       "70707  2022-01-30  23:36:34  57.213.70.113       GET  transactions.aspx   \n",
       "70708  2022-01-30  23:36:46  57.213.70.113       GET       template.css   \n",
       "70709  2022-01-30  23:36:46  57.213.70.113       GET  transactions.aspx   \n",
       "\n",
       "      cs-uri-query  s-port cs-username            c-ip  \\\n",
       "0       v=uwixfkca     443           -  92.123.193.245   \n",
       "1       v=igohhmua     443           -  92.123.193.245   \n",
       "2                -     443           -  92.123.193.245   \n",
       "3                -     443           -  92.123.193.245   \n",
       "4                -     443           -  92.123.193.245   \n",
       "...            ...     ...         ...             ...   \n",
       "70705            -     443    ry814066  194.202.68.159   \n",
       "70706     v=599940     443    ry814066  194.202.68.159   \n",
       "70707       page=3     443    ry814066  194.202.68.159   \n",
       "70708   v=tvjgabsg     443    ry814066  194.202.68.159   \n",
       "70709       page=4     443    ry814066  194.202.68.159   \n",
       "\n",
       "                                          cs(User-Agent)  \\\n",
       "0      Mozilla/4.0+(compatible;+MSIE+6.0;+Windows+NT+...   \n",
       "1      Mozilla/4.0+(compatible;+MSIE+6.0;+Windows+NT+...   \n",
       "2      Mozilla/4.0+(compatible;+MSIE+6.0;+Windows+NT+...   \n",
       "3      Mozilla/4.0+(compatible;+MSIE+6.0;+Windows+NT+...   \n",
       "4      Mozilla/4.0+(compatible;+MSIE+6.0;+Windows+NT+...   \n",
       "...                                                  ...   \n",
       "70705  Mozilla/5.0+(iPhone;+CPU+iPhone+OS+12_4_1+like...   \n",
       "70706  Mozilla/5.0+(iPhone;+CPU+iPhone+OS+12_4_1+like...   \n",
       "70707  Mozilla/5.0+(iPhone;+CPU+iPhone+OS+12_4_1+like...   \n",
       "70708  Mozilla/5.0+(iPhone;+CPU+iPhone+OS+12_4_1+like...   \n",
       "70709  Mozilla/5.0+(iPhone;+CPU+iPhone+OS+12_4_1+like...   \n",
       "\n",
       "                                      cs(Referer)  sc-status  sc-substatus  \\\n",
       "0                                               -        200             0   \n",
       "1                                               -        200             0   \n",
       "2                                               -        404             0   \n",
       "3                                               -        200             0   \n",
       "4             https://bankofpunk.local/index.aspx        200             0   \n",
       "...                                           ...        ...           ...   \n",
       "70705  https://bankofpunk.local/transactions.aspx        200             0   \n",
       "70706  https://bankofpunk.local/transactions.aspx        200             0   \n",
       "70707  https://bankofpunk.local/transactions.aspx        200             0   \n",
       "70708  https://bankofpunk.local/transactions.aspx        200             0   \n",
       "70709  https://bankofpunk.local/transactions.aspx        200             0   \n",
       "\n",
       "       sc-win32-status  time-taken  \n",
       "0                    0          24  \n",
       "1                    0          21  \n",
       "2                    0          26  \n",
       "3                    0          26  \n",
       "4                    0          24  \n",
       "...                ...         ...  \n",
       "70705                0          28  \n",
       "70706                0          30  \n",
       "70707                0          22  \n",
       "70708                0          20  \n",
       "70709                0          25  \n",
       "\n",
       "[70710 rows x 15 columns]"
      ]
     },
     "execution_count": 2,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Import libraries as required\n",
    "import pandas as pd\n",
    "import numpy as np\n",
    "import matplotlib.pyplot as plt\n",
    "pd.set_option('display.max_rows', 10)\n",
    "\n",
    "data_file = 'ao2-oladapo'\n",
    "\n",
    "\n",
    "# Load in the data set as required - \n",
    "data_path = 'C:\\Users\\lordj\\Desktop\\Jupyter'\n",
    "data = pd.read_csv(data_path + data_file, delim_whitespace=True)\n",
    "#data.to_csv('out.csv')\n",
    "temp_df = data[data.columns[:-1]]\n",
    "temp_df.columns = data.columns[1:]\n",
    "data = temp_df\n",
    "data"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Index(['date', 'time', 's-ip', 'cs-method', 'cs-uri-stem', 'cs-uri-query',\n",
       "       's-port', 'cs-username', 'c-ip', 'cs(User-Agent)', 'cs(Referer)',\n",
       "       'sc-status', 'sc-substatus', 'sc-win32-status', 'time-taken'],\n",
       "      dtype='object')"
      ]
     },
     "execution_count": 2,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Get all column names\n",
    "data.columns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array(['-', 'https://bankofpunk.local/index.aspx',\n",
       "       'https://bankofpunk.local/login.aspx',\n",
       "       'https://bankofpunk.local/account_status.aspx',\n",
       "       'https://bankofpunk.local/transactions.aspx',\n",
       "       'https://bankofpunk.local/transfer.aspx',\n",
       "       'https://bankofpunk.local/faq.aspx',\n",
       "       'https://bankofpunk.local/transfer_complete.aspx',\n",
       "       'https://bankofpunk.local/changepassword.aspx',\n",
       "       'https://bankofpunk.local/change_avatar.aspx'], dtype=object)"
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Search for all unique entries in 'cs(Referer)'\n",
    "data['cs(Referer)'].unique()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "https://bankofpunk.local/transactions.aspx         23346\n",
       "https://bankofpunk.local/index.aspx                18160\n",
       "https://bankofpunk.local/login.aspx                13718\n",
       "https://bankofpunk.local/account_status.aspx        6228\n",
       "-                                                   5964\n",
       "https://bankofpunk.local/transfer.aspx              1924\n",
       "https://bankofpunk.local/changepassword.aspx         617\n",
       "https://bankofpunk.local/faq.aspx                    354\n",
       "https://bankofpunk.local/transfer_complete.aspx      305\n",
       "https://bankofpunk.local/change_avatar.aspx           94\n",
       "Name: cs(Referer), dtype: int64"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Get count of each unique value for 'cs(Referer)'\n",
    "data['cs(Referer)'].value_counts()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 1440x360 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "# Plot the first 100 values for the 'time-taken' column\n",
    "plt.figure(figsize=(20,5))\n",
    "plt.plot(data['time-taken'][:100])\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "GET     67386\n",
       "POST     3324\n",
       "Name: cs-method, dtype: int64"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data['cs-method'].value_counts()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "**I started my investigation** by retrieving a summary of the HTTP methods avaialable in the logs, this allowed me to determine the sort of actions being performed, there are two methods in use\n",
    "- POST: This implies that data is being submitted to the web server\n",
    "- GET: This implies that information is being requested from the web server"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "443    70710\n",
       "Name: s-port, dtype: int64"
      ]
     },
     "execution_count": 18,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data['s-port'].value_counts()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Retrieval of a summary on the ports in the log to determine if any request was transmitted via HTTP, the presence of only port \"443\" indicates that all queries were sent using the secure protocol, HTTPS."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 62,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "200    59292\n",
       "404     5901\n",
       "301     4647\n",
       "401      870\n",
       "Name: sc-status, dtype: int64"
      ]
     },
     "execution_count": 62,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data['sc-status'].value_counts()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "To determine the presence of strange response codes or multiple unauthorised attempts, a summary of the HTTP status codes existing in the log is queried."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "149.5.250.189     287\n",
       "192.151.139.83    215\n",
       "195.12.35.175     201\n",
       "185.227.52.167    148\n",
       "193.201.92.65     144\n",
       "57.246.139.228    141\n",
       "83.146.21.50      140\n",
       "146.70.35.29      140\n",
       "185.144.222.67    135\n",
       "80.75.221.83      131\n",
       "Name: c-ip, dtype: int64"
      ]
     },
     "execution_count": 2,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data['c-ip'].value_counts().head(n=10)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The top 10 IP addresses are listed in order of the number of times they appear in the log, which indicates how many times they've made a request to the web server. This is to see whether any of the IP addresses are sending an unusually large number of requests."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "sc-status    200   301  401   404\n",
      "cs-method                        \n",
      "GET        58975  2510    0  5901\n",
      "POST         317  2137  870     0\n"
     ]
    }
   ],
   "source": [
    "group_method_st= pd.crosstab(data['cs-method'],data['sc-status'])\n",
    "print(group_method_st)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Checking HTTP methods against status codes. It demonstrates that there are some 401 post requests."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "cs-method       GET  POST\n",
      "c-ip                     \n",
      "149.5.250.189    66   221\n",
      "192.151.139.83   65   150\n",
      "195.12.35.175    71   130\n",
      "83.146.21.50    131     9\n",
      "194.63.118.55    75     9\n",
      "192.189.41.151  117     9\n",
      "88.214.51.233   113     8\n",
      "178.21.35.137    98     8\n",
      "212.90.113.10   103     8\n",
      "192.77.138.137  119     8\n"
     ]
    }
   ],
   "source": [
    "table_grouping_method= pd.crosstab(data['c-ip'],data['cs-method'])\n",
    "sorted_grouping_method=table_grouping_method.sort_values(by='POST', ascending=False).head(n=10)\n",
    "print(sorted_grouping_method)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "An attempt to idenitfy the likely source client ip addresses that are sending the unauthorized post requests. \n",
    "<br>\n",
    "The IP addresses below have the highest counts of POST requests\n",
    "1. 149.5.250.189    221\n",
    "2. 192.151.139.83   150\n",
    "3. 195.12.35.175    130"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 18,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "image/png": "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4P6ytXZt/z5emq5d/jjJU1prP5hn+Rtc65zVPwtr+rjL0/XO+tMk16TrMfTyrL1OvdP6mMXi3pPk/Uk+l67db0ny+/2yLkz3jX4fTNdb6IdJ1ozM+ytJLqyqH/fLfXZ/yxsAbHaqtbn2wgUAAABgU6aHEAAAAMDACIQAAAAABkYgBAAAADAwAiEAAACAgdlyqSuQJDvvvHNbvnz5UlcDAAAAYLNx7rnn/qC1tsu4cRtFILR8+fKcc845S10NAAAAgM1GVV02aZxbxgAAAAAGRiAEAAAAMDACIQAAAICB2SieIQQAAACwEH72s59lzZo1ueWWW5a6KhvMNttskz322CNbbbXVrOcRCAEAAACbjTVr1uQe97hHli9fnqpa6uosutZarr322qxZsyZ77733rOdzyxgAAACw2bjlllty73vfexBhUJJUVe5973vPuUeUQAgAAADYrAwlDJoyn/crEAIAAAAYGIEQAAAAwCJ4wxvecMfr1atX5yEPeci8y1rf+acTCAEAAAAsgtFAaGMjEAIAAAA2e6eeemoe9rCH5YADDsiRRx6Zj33sY3nIQx6SAw44II9//OPHznPwwQfnuOOOy8qVK7Pvvvvmq1/9ao444ojss88+edWrXnXHdB/4wAfyqEc9KitWrMjv/u7v5rbbbsvxxx+fm2++OStWrMhzn/vcJMltt92W3/md38n++++fQw89NDfffHOSZNWqVTnooIPysIc9LE9/+tPzwx/+MEly7rnn5oADDsgBBxyQk08+eUHXh0AIAAAA2KxdeOGFed3rXpezzz475513Xt7+9rfnNa95TT7zmc/kvPPOy+mnnz5x3mXLluWcc87JMccck8MPPzwnn3xyLrjggrzvfe/Ltddem4suuigf+chH8sUvfjGrVq3KFltskdNOOy0nnnhitt1226xatSqnnXZakuTiiy/OsccemwsvvDA77rhjPvGJTyRJnve85+Wkk07K+eefn4c+9KF59atfnSR5wQtekL/6q7/Keeedt+DrRCAEAAAAbNbOPvvs/Pqv/3p23nnnJMm97nWv/OIv/mKe//zn52//9m9z2223TZz3qU99apLkoQ99aPbff//suuuu2XrrrXP/+98/l19+ec4666yce+65OfDAA7NixYqcddZZufTSS8eWtffee2fFihVJkkc+8pFZvXp1brjhhlx//fV5whOekCQ56qij8rnPfS7XX399rr/++jt6Lx155JELtTqSJFsuaGkAAAAAm4B3vvOd+fKXv5wzzjgjj3zkI3PuuefmZS97Wf7rv/4ru+22W84888wkydZbb50kudvd7nbH66m/b7311rTWctRRR+WNb3zjOpc5Ov8WW2xxxy1jS0EPIQAAAGCz9sQnPjEf+9jHcu211yZJrrvuunznO9/Jox/96LzmNa/JLrvskssvvzzvfe97s2rVqjvCoNk45JBD8vGPfzxXX331HWVfdtllSZKtttoqP/vZz2acf4cddshOO+2Uz3/+80mS97///XnCE56QHXfcMTvuuGO+8IUvJMkdt50tFD2EAAAAgM3a/vvvn1e+8pV5whOekC222CIPf/jDc+ONN+biiy9Oay2HHHJIDjjggHmVvd9+++V1r3tdDj300Nx+++3ZaqutcvLJJ2evvfbK0UcfnYc97GF5xCMekde//vUTyzjllFNyzDHH5Kabbsr973//vPe9702SvPe9780LX/jCVFUOPfTQedVvkmqtLWiB87Fy5cp2zjnnLHU1AAAAgE3cRRddlH333Xepq7HBjXvfVXVua23luOndMgYAAAAwMAIhAAAAgIERCAEAAAAMjEAIAAAAYGAEQgAAAAADIxACAAAAGJgtl7oCAAAAAItl+fFnLGh5q088bFbTff/7389xxx2XL33pS9lpp52ybNmy/PEf/3F22mmnHH744dl7773vmPYVr3hF3vjGNyZJrrrqqmyxxRbZZZddkiRf+cpXsmzZsgV9D4lAaMM4YYcNvLwbNuzyAAAAgDu01vK0pz0tRx11VD74wQ8mSS677LKcfvrp2WmnnfK4xz0un/70p+80z2/8xm8kSU444YRsv/32ednLXraodXTLGAAAAMACOvvss7Ns2bIcc8wxdwzba6+98vu///tLWKs700MIAAAAYAFdeOGFecQjHjFx/Oc///msWLHijr8/8YlP5AEPeMAGqNlaAiEAAACARXTsscfmC1/4QpYtW5Y3v/nNY28Z29DcMgYAAACwgPbff/987Wtfu+Pvk08+OWeddVauueaaJazVnQmEAAAAABbQE5/4xNxyyy15xzveccewm266aQlrdFduGQMAAAA2W7P9mviFVFX51Kc+leOOOy5vetObsssuu+Tud797TjrppCR3fYbQq171qjzzmc/coHUUCAEAAAAssF133TUf/vCHx4674YYbJs53wgknLFKN7swtYwAAAAADIxACAAAAGBiBEAAAAMDACIQAAAAABkYgBAAAADAwAiEAAACAgfG18wAAAMDm64QdFri8yV8ZP2WLLbbIQx/60Nx6663Zd999c8opp2S77bbLmjVrcuyxx+Yb3/hGbr/99jzlKU/Jm9/85ixbtiw33XRTfud3fifnn39+WmvZcccdc9ppp+Xwww9Pklx11VXZYostsssuuyRJvvKVr2TZsmXzfht6CAEAAAAsoG233TarVq3KBRdckGXLluWd73xnWms54ogj8rSnPS0XX3xxvv3tb+fHP/5xXvnKVyZJ3v72t+e+971vvv71r+eCCy7Iu9/97vzcz/1cVq1alVWrVuWYY47Jcccdd8ff6xMGJQIhAAAAgEXzuMc9LpdccknOPvvsbLPNNnnBC16QpOtF9Na3vjXvec97ctNNN+XKK6/M7rvvfsd8D37wg7P11lsvWr0EQgAAAACL4NZbb80//dM/5aEPfWguvPDCPPKRj7zT+Hve857Zc889c8kll+SFL3xhTjrppDzmMY/Jq171qlx88cWLWjeBEAAAAMACuvnmm7NixYqsXLkye+65Z170ohetc54VK1bk0ksvzctf/vJcd911OfDAA3PRRRctWh09VBoAAABgAU09Q2jUfvvtl49//ON3GnbjjTfme9/7Xh74wAcmSbbffvscccQROeKII3K3u90tZ555Zvbdd99FqaMeQgAAAACL7JBDDslNN92UU089NUly22235aUvfWme//znZ7vttssXv/jF/PCHP0yS/PSnP803vvGN7LXXXotWHz2EAAAAgM3XLL4mfkOoqnzyk5/Mi1/84rz2ta/N7bffnic/+cl5wxvekCT5zne+k9/7vd9Lay233357DjvssDzjGc9YvPq01hat8NlauXJlO+ecc5a6GovnhB028PI2jo0dAAAANrSLLrpo0W6z2piNe99VdW5rbeW46d0yBgAAADAwAiEAAACAgREIAQAAAJuVjeHxOBvSfN6vQAgAAADYbGyzzTa59tprBxMKtdZy7bXXZptttpnTfL5lDAAAANhs7LHHHlmzZk2uueaapa7KBrPNNttkjz32mNM8AiEAAABgs7HVVltl7733XupqbPTcMgYAAAAwMAIhAAAAgIERCAEAAAAM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\n",
      "text/plain": [
       "<Figure size 1440x720 with 1 Axes>"
      ]
     },
     "metadata": {
      "needs_background": "light"
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "sorted_grouping_method.plot(kind='bar', rot=0, title=\"IP Addresses In Relation to HTTP Methods\", figsize=(20,10)).grid(False)\n",
    "plt.show()"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "This bar chart shows a visualization of the top ten client ip addresses in terms of POST requests"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 87,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "                c-ip  sc-status  counts\n",
      "275    149.5.250.189        401     214\n",
      "1090  192.151.139.83        401     143\n",
      "1492   195.12.35.175        401     123\n",
      "1451   194.63.118.55        401       6\n",
      "1110  192.189.41.151        401       5\n",
      "2841    83.146.21.50        401       5\n",
      "558    178.21.35.137        401       5\n",
      "2978    89.197.59.19        401       5\n",
      "2943    87.238.78.13        401       4\n",
      "1019  188.119.155.16        401       4\n"
     ]
    }
   ],
   "source": [
    "analysis_frame_group=data.groupby(['c-ip', 'sc-status']).size()\n",
    "\n",
    "count_status=analysis_frame_group.to_frame(name=\"counts\").reset_index()\n",
    "\n",
    "count_401_sorted = (count_status[(count_401['sc-status']==401)]).sort_values(by='counts', ascending = False).head(n=10)\n",
    "print(count_401_sorted)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 401\n",
    "This summary shows that the IP addresses below have the highest number of unauthorized attempt status code (401), however that is not enough to draw any conclusion as the attempts can be valid attempts over a long a period of time\n",
    "<br>\n",
    "\n",
    "1. 149.5.250.189         214\n",
    "2. 192.151.139.83        143\n",
    "3. 195.12.35.175         123"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 85,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "             date      time           c-ip  sc-status  counts\n",
      "23934  2022-01-22  12:15:19  149.5.250.189        401       1\n",
      "23935  2022-01-22  12:15:22  149.5.250.189        401       1\n",
      "23936  2022-01-22  12:15:24  149.5.250.189        401       1\n",
      "23937  2022-01-22  12:15:27  149.5.250.189        401       1\n",
      "23938  2022-01-22  12:15:30  149.5.250.189        401       1\n",
      "23939  2022-01-22  12:15:33  149.5.250.189        401       1\n",
      "23940  2022-01-22  12:15:35  149.5.250.189        401       1\n",
      "23941  2022-01-22  12:15:37  149.5.250.189        401       1\n",
      "23942  2022-01-22  12:15:39  149.5.250.189        401       1\n",
      "23943  2022-01-22  12:15:40  149.5.250.189        401       1\n",
      "23946  2022-01-22  12:15:43  149.5.250.189        401       1\n",
      "23947  2022-01-22  12:15:44  149.5.250.189        401       1\n",
      "23948  2022-01-22  12:15:46  149.5.250.189        401       1\n",
      "23949  2022-01-22  12:15:47  149.5.250.189        401       1\n",
      "23950  2022-01-22  12:15:49  149.5.250.189        401       1\n",
      "23951  2022-01-22  12:15:52  149.5.250.189        401       1\n",
      "23952  2022-01-22  12:15:53  149.5.250.189        401       1\n",
      "23953  2022-01-22  12:15:56  149.5.250.189        401       1\n",
      "23954  2022-01-22  12:15:57  149.5.250.189        401       1\n",
      "23955  2022-01-22  12:16:00  149.5.250.189        401       1\n",
      "23956  2022-01-22  12:16:02  149.5.250.189        401       1\n",
      "23957  2022-01-22  12:16:03  149.5.250.189        401       1\n",
      "23958  2022-01-22  12:16:04  149.5.250.189        401       1\n",
      "23959  2022-01-22  12:16:05  149.5.250.189        401       1\n",
      "23960  2022-01-22  12:16:06  149.5.250.189        401       1\n",
      "23961  2022-01-22  12:16:07  149.5.250.189        401       1\n",
      "23963  2022-01-22  12:16:10  149.5.250.189        401       1\n",
      "23964  2022-01-22  12:16:12  149.5.250.189        401       1\n",
      "23965  2022-01-22  12:16:13  149.5.250.189        401       1\n",
      "23966  2022-01-22  12:16:15  149.5.250.189        401       1\n",
      "23967  2022-01-22  12:16:16  149.5.250.189        401       1\n",
      "23968  2022-01-22  12:16:18  149.5.250.189        401       1\n",
      "23969  2022-01-22  12:16:19  149.5.250.189        401       1\n",
      "23970  2022-01-22  12:16:21  149.5.250.189        401       1\n",
      "23971  2022-01-22  12:16:23  149.5.250.189        401       1\n",
      "23974  2022-01-22  12:16:25  149.5.250.189        401       1\n",
      "23975  2022-01-22  12:16:28  149.5.250.189        401       1\n",
      "23976  2022-01-22  12:16:29  149.5.250.189        401       1\n",
      "23977  2022-01-22  12:16:31  149.5.250.189        401       1\n",
      "23978  2022-01-22  12:16:34  149.5.250.189        401       1\n",
      "23979  2022-01-22  12:16:36  149.5.250.189        401       1\n",
      "23980  2022-01-22  12:16:39  149.5.250.189        401       1\n",
      "23981  2022-01-22  12:16:40  149.5.250.189        401       1\n",
      "23982  2022-01-22  12:16:41  149.5.250.189        401       1\n",
      "23983  2022-01-22  12:16:43  149.5.250.189        401       1\n",
      "23986  2022-01-22  12:16:46  149.5.250.189        401       1\n",
      "23987  2022-01-22  12:16:48  149.5.250.189        401       1\n",
      "23988  2022-01-22  12:16:50  149.5.250.189        401       1\n",
      "23989  2022-01-22  12:16:51  149.5.250.189        401       1\n",
      "23990  2022-01-22  12:16:53  149.5.250.189        401       1\n",
      "23991  2022-01-22  12:16:56  149.5.250.189        401       1\n",
      "23993  2022-01-22  12:16:59  149.5.250.189        401       1\n",
      "23994  2022-01-22  12:17:00  149.5.250.189        401       1\n",
      "23996  2022-01-22  12:17:03  149.5.250.189        401       1\n",
      "23998  2022-01-22  12:17:04  149.5.250.189        401       1\n",
      "23999  2022-01-22  12:17:05  149.5.250.189        401       1\n",
      "24000  2022-01-22  12:17:06  149.5.250.189        401       1\n",
      "24001  2022-01-22  12:17:07  149.5.250.189        401       1\n",
      "24002  2022-01-22  12:17:10  149.5.250.189        401       1\n",
      "24003  2022-01-22  12:17:13  149.5.250.189        401       1\n",
      "24004  2022-01-22  12:17:15  149.5.250.189        401       1\n",
      "24005  2022-01-22  12:17:16  149.5.250.189        401       1\n",
      "24006  2022-01-22  12:17:19  149.5.250.189        401       1\n",
      "24008  2022-01-22  12:17:22  149.5.250.189        401       1\n",
      "24009  2022-01-22  12:17:25  149.5.250.189        401       1\n",
      "24011  2022-01-22  12:17:27  149.5.250.189        401       1\n",
      "24012  2022-01-22  12:17:29  149.5.250.189        401       1\n",
      "24014  2022-01-22  12:17:31  149.5.250.189        401       1\n",
      "24015  2022-01-22  12:17:32  149.5.250.189        401       1\n",
      "24016  2022-01-22  12:17:33  149.5.250.189        401       1\n",
      "24017  2022-01-22  12:17:34  149.5.250.189        401       1\n",
      "24018  2022-01-22  12:17:36  149.5.250.189        401       1\n",
      "24019  2022-01-22  12:17:39  149.5.250.189        401       1\n",
      "24020  2022-01-22  12:17:41  149.5.250.189        401       1\n",
      "24021  2022-01-22  12:17:44  149.5.250.189        401       1\n",
      "24022  2022-01-22  12:17:47  149.5.250.189        401       1\n",
      "24023  2022-01-22  12:17:50  149.5.250.189        401       1\n",
      "24024  2022-01-22  12:17:51  149.5.250.189        401       1\n",
      "24026  2022-01-22  12:17:53  149.5.250.189        401       1\n",
      "24027  2022-01-22  12:17:56  149.5.250.189        401       1\n",
      "24031  2022-01-22  12:17:58  149.5.250.189        401       1\n",
      "24032  2022-01-22  12:18:01  149.5.250.189        401       1\n",
      "24033  2022-01-22  12:18:04  149.5.250.189        401       1\n",
      "24034  2022-01-22  12:18:06  149.5.250.189        401       1\n",
      "24035  2022-01-22  12:18:07  149.5.250.189        401       1\n",
      "24036  2022-01-22  12:18:08  149.5.250.189        401       1\n",
      "24037  2022-01-22  12:18:11  149.5.250.189        401       1\n",
      "24038  2022-01-22  12:18:12  149.5.250.189        401       1\n",
      "24039  2022-01-22  12:18:14  149.5.250.189        401       1\n",
      "24040  2022-01-22  12:18:15  149.5.250.189        401       1\n",
      "24041  2022-01-22  12:18:16  149.5.250.189        401       1\n",
      "24043  2022-01-22  12:18:19  149.5.250.189        401       1\n",
      "24045  2022-01-22  12:18:22  149.5.250.189        401       1\n",
      "24046  2022-01-22  12:18:24  149.5.250.189        401       1\n",
      "24047  2022-01-22  12:18:25  149.5.250.189        401       1\n",
      "24048  2022-01-22  12:18:27  149.5.250.189        401       1\n",
      "24049  2022-01-22  12:18:28  149.5.250.189        401       1\n",
      "24050  2022-01-22  12:18:29  149.5.250.189        401       1\n",
      "24051  2022-01-22  12:18:32  149.5.250.189        401       1\n",
      "24052  2022-01-22  12:18:34  149.5.250.189        401       1\n",
      "24053  2022-01-22  12:18:36  149.5.250.189        401       1\n",
      "24054  2022-01-22  12:18:38  149.5.250.189        401       1\n",
      "24059  2022-01-22  12:18:41  149.5.250.189        401       1\n",
      "24060  2022-01-22  12:18:42  149.5.250.189        401       1\n",
      "24061  2022-01-22  12:18:45  149.5.250.189        401       1\n",
      "24062  2022-01-22  12:18:47  149.5.250.189        401       1\n",
      "24063  2022-01-22  12:18:48  149.5.250.189        401       1\n",
      "24064  2022-01-22  12:18:50  149.5.250.189        401       1\n",
      "24065  2022-01-22  12:18:52  149.5.250.189        401       1\n",
      "24067  2022-01-22  12:18:53  149.5.250.189        401       1\n",
      "24068  2022-01-22  12:18:56  149.5.250.189        401       1\n",
      "24069  2022-01-22  12:18:57  149.5.250.189        401       1\n",
      "24070  2022-01-22  12:19:00  149.5.250.189        401       1\n",
      "24071  2022-01-22  12:19:02  149.5.250.189        401       1\n",
      "24072  2022-01-22  12:19:03  149.5.250.189        401       1\n",
      "24073  2022-01-22  12:19:05  149.5.250.189        401       1\n",
      "24076  2022-01-22  12:19:06  149.5.250.189        401       1\n",
      "24077  2022-01-22  12:19:09  149.5.250.189        401       1\n",
      "24078  2022-01-22  12:19:11  149.5.250.189        401       1\n",
      "24079  2022-01-22  12:19:14  149.5.250.189        401       1\n",
      "24080  2022-01-22  12:19:17  149.5.250.189        401       1\n",
      "24081  2022-01-22  12:19:19  149.5.250.189        401       1\n",
      "24083  2022-01-22  12:19:20  149.5.250.189        401       1\n",
      "24084  2022-01-22  12:19:21  149.5.250.189        401       1\n",
      "24085  2022-01-22  12:19:22  149.5.250.189        401       1\n",
      "24086  2022-01-22  12:19:25  149.5.250.189        401       1\n",
      "24088  2022-01-22  12:19:27  149.5.250.189        401       1\n",
      "24089  2022-01-22  12:19:30  149.5.250.189        401       1\n",
      "24090  2022-01-22  12:19:33  149.5.250.189        401       1\n",
      "24091  2022-01-22  12:19:36  149.5.250.189        401       1\n",
      "24093  2022-01-22  12:19:37  149.5.250.189        401       1\n",
      "24094  2022-01-22  12:19:40  149.5.250.189        401       1\n",
      "24095  2022-01-22  12:19:41  149.5.250.189        401       1\n",
      "24096  2022-01-22  12:19:42  149.5.250.189        401       1\n",
      "24097  2022-01-22  12:19:44  149.5.250.189        401       1\n",
      "24098  2022-01-22  12:19:47  149.5.250.189        401       1\n",
      "24099  2022-01-22  12:19:49  149.5.250.189        401       1\n",
      "24100  2022-01-22  12:19:51  149.5.250.189        401       1\n",
      "24101  2022-01-22  12:19:54  149.5.250.189        401       1\n",
      "24102  2022-01-22  12:19:56  149.5.250.189        401       1\n",
      "24103  2022-01-22  12:19:58  149.5.250.189        401       1\n",
      "24104  2022-01-22  12:20:00  149.5.250.189        401       1\n",
      "24105  2022-01-22  12:20:02  149.5.250.189        401       1\n",
      "24107  2022-01-22  12:20:04  149.5.250.189        401       1\n",
      "24108  2022-01-22  12:20:05  149.5.250.189        401       1\n",
      "24109  2022-01-22  12:20:07  149.5.250.189        401       1\n",
      "24110  2022-01-22  12:20:10  149.5.250.189        401       1\n",
      "24111  2022-01-22  12:20:11  149.5.250.189        401       1\n",
      "24112  2022-01-22  12:20:12  149.5.250.189        401       1\n",
      "24114  2022-01-22  12:20:15  149.5.250.189        401       1\n",
      "24115  2022-01-22  12:20:17  149.5.250.189        401       1\n",
      "24116  2022-01-22  12:20:18  149.5.250.189        401       1\n",
      "24117  2022-01-22  12:20:19  149.5.250.189        401       1\n",
      "24118  2022-01-22  12:20:20  149.5.250.189        401       1\n",
      "24120  2022-01-22  12:20:23  149.5.250.189        401       1\n",
      "24121  2022-01-22  12:20:24  149.5.250.189        401       1\n",
      "24122  2022-01-22  12:20:27  149.5.250.189        401       1\n",
      "24123  2022-01-22  12:20:28  149.5.250.189        401       1\n",
      "24124  2022-01-22  12:20:30  149.5.250.189        401       1\n",
      "24125  2022-01-22  12:20:33  149.5.250.189        401       1\n",
      "24126  2022-01-22  12:20:35  149.5.250.189        401       1\n",
      "24127  2022-01-22  12:20:38  149.5.250.189        401       1\n",
      "24128  2022-01-22  12:20:41  149.5.250.189        401       1\n",
      "24129  2022-01-22  12:20:42  149.5.250.189        401       1\n",
      "24130  2022-01-22  12:20:43  149.5.250.189        401       1\n",
      "24131  2022-01-22  12:20:46  149.5.250.189        401       1\n",
      "24132  2022-01-22  12:20:49  149.5.250.189        401       1\n",
      "24133  2022-01-22  12:20:51  149.5.250.189        401       1\n",
      "24134  2022-01-22  12:20:54  149.5.250.189        401       1\n",
      "24135  2022-01-22  12:20:57  149.5.250.189        401       1\n",
      "24136  2022-01-22  12:20:59  149.5.250.189        401       1\n",
      "24137  2022-01-22  12:21:02  149.5.250.189        401       1\n",
      "24138  2022-01-22  12:21:05  149.5.250.189        401       1\n",
      "24139  2022-01-22  12:21:07  149.5.250.189        401       1\n",
      "24140  2022-01-22  12:21:08  149.5.250.189        401       1\n",
      "24141  2022-01-22  12:21:11  149.5.250.189        401       1\n",
      "24142  2022-01-22  12:21:14  149.5.250.189        401       1\n",
      "24143  2022-01-22  12:21:17  149.5.250.189        401       1\n",
      "24144  2022-01-22  12:21:18  149.5.250.189        401       1\n",
      "24145  2022-01-22  12:21:21  149.5.250.189        401       1\n",
      "24146  2022-01-22  12:21:22  149.5.250.189        401       1\n",
      "24147  2022-01-22  12:21:25  149.5.250.189        401       1\n",
      "24148  2022-01-22  12:21:28  149.5.250.189        401       1\n",
      "24149  2022-01-22  12:21:30  149.5.250.189        401       1\n",
      "24150  2022-01-22  12:21:33  149.5.250.189        401       1\n",
      "24151  2022-01-22  12:21:35  149.5.250.189        401       1\n",
      "24152  2022-01-22  12:21:37  149.5.250.189        401       1\n",
      "24153  2022-01-22  12:21:40  149.5.250.189        401       1\n",
      "24154  2022-01-22  12:21:43  149.5.250.189        401       1\n",
      "24155  2022-01-22  12:21:46  149.5.250.189        401       1\n",
      "24156  2022-01-22  12:21:48  149.5.250.189        401       1\n",
      "24157  2022-01-22  12:21:51  149.5.250.189        401       1\n",
      "24158  2022-01-22  12:21:53  149.5.250.189        401       1\n",
      "24159  2022-01-22  12:21:55  149.5.250.189        401       1\n",
      "24160  2022-01-22  12:21:58  149.5.250.189        401       1\n",
      "24161  2022-01-22  12:22:01  149.5.250.189        401       1\n",
      "24162  2022-01-22  12:22:04  149.5.250.189        401       1\n",
      "24163  2022-01-22  12:22:05  149.5.250.189        401       1\n",
      "24164  2022-01-22  12:22:07  149.5.250.189        401       1\n",
      "24165  2022-01-22  12:22:09  149.5.250.189        401       1\n",
      "24166  2022-01-22  12:22:12  149.5.250.189        401       1\n",
      "24167  2022-01-22  12:22:13  149.5.250.189        401       1\n",
      "24168  2022-01-22  12:22:16  149.5.250.189        401       1\n",
      "24169  2022-01-22  12:22:17  149.5.250.189        401       1\n",
      "24170  2022-01-22  12:22:19  149.5.250.189        401       1\n",
      "24171  2022-01-22  12:22:22  149.5.250.189        401       1\n",
      "24172  2022-01-22  12:22:25  149.5.250.189        401       1\n",
      "24173  2022-01-22  12:22:27  149.5.250.189        401       1\n",
      "24174  2022-01-22  12:22:28  149.5.250.189        401       1\n",
      "24175  2022-01-22  12:22:31  149.5.250.189        401       1\n",
      "24176  2022-01-22  12:22:33  149.5.250.189        401       1\n",
      "24177  2022-01-22  12:22:34  149.5.250.189        401       1\n",
      "24178  2022-01-22  12:22:35  149.5.250.189        401       1\n",
      "24179  2022-01-22  12:22:36  149.5.250.189        401       1\n"
     ]
    }
   ],
   "source": [
    "pd.set_option('display.max_rows', None)\n",
    "time_frame_group=data.groupby(['date','time','c-ip', 'sc-status']).size()\n",
    "\n",
    "count_time_frame=time_frame_group.to_frame(name=\"counts\").reset_index()\n",
    "\n",
    "sip1=(count_time_frame['sc-status']==401)&(count_time_frame['c-ip']=='149.5.250.189')\n",
    "\n",
    "print(count_time_frame[sip1])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "This summary shows a rundown of the unauthorized attempts from the ip adress **149.5.250.189** in relation to the time that the attempts were made. <br>\n",
    "The attempts were made on the same day with an average of 1-2 seconds time difference between each attempts "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "            date      time            c-ip  sc-status  counts\n",
      "1219  2022-01-01  21:27:24  192.151.139.83        401       1\n",
      "1220  2022-01-01  21:27:25  192.151.139.83        401       1\n",
      "1223  2022-01-01  21:27:27  192.151.139.83        401       1\n",
      "1224  2022-01-01  21:27:29  192.151.139.83        401       1\n",
      "1225  2022-01-01  21:27:31  192.151.139.83        401       1\n",
      "1226  2022-01-01  21:27:32  192.151.139.83        401       1\n",
      "1227  2022-01-01  21:27:34  192.151.139.83        401       1\n",
      "1230  2022-01-01  21:27:36  192.151.139.83        401       1\n",
      "1231  2022-01-01  21:27:38  192.151.139.83        401       1\n",
      "1234  2022-01-01  21:27:41  192.151.139.83        401       1\n",
      "1235  2022-01-01  21:27:44  192.151.139.83        401       1\n",
      "1236  2022-01-01  21:27:45  192.151.139.83        401       1\n",
      "1237  2022-01-01  21:27:48  192.151.139.83        401       1\n",
      "1238  2022-01-01  21:27:50  192.151.139.83        401       1\n",
      "1239  2022-01-01  21:27:53  192.151.139.83        401       1\n",
      "1240  2022-01-01  21:27:54  192.151.139.83        401       1\n",
      "1241  2022-01-01  21:27:55  192.151.139.83        401       1\n",
      "1242  2022-01-01  21:27:57  192.151.139.83        401       1\n",
      "1243  2022-01-01  21:28:00  192.151.139.83        401       1\n",
      "1244  2022-01-01  21:28:02  192.151.139.83        401       1\n",
      "1247  2022-01-01  21:28:05  192.151.139.83        401       1\n",
      "1248  2022-01-01  21:28:07  192.151.139.83        401       1\n",
      "1249  2022-01-01  21:28:08  192.151.139.83        401       1\n",
      "1250  2022-01-01  21:28:10  192.151.139.83        401       1\n",
      "1251  2022-01-01  21:28:12  192.151.139.83        401       1\n",
      "1252  2022-01-01  21:28:14  192.151.139.83        401       1\n",
      "1253  2022-01-01  21:28:17  192.151.139.83        401       1\n",
      "1254  2022-01-01  21:28:19  192.151.139.83        401       1\n",
      "1256  2022-01-01  21:28:22  192.151.139.83        401       1\n",
      "1257  2022-01-01  21:28:25  192.151.139.83        401       1\n",
      "1258  2022-01-01  21:28:26  192.151.139.83        401       1\n",
      "1259  2022-01-01  21:28:28  192.151.139.83        401       1\n",
      "1260  2022-01-01  21:28:30  192.151.139.83        401       1\n",
      "1262  2022-01-01  21:28:32  192.151.139.83        401       1\n",
      "1263  2022-01-01  21:28:34  192.151.139.83        401       1\n",
      "1265  2022-01-01  21:28:37  192.151.139.83        401       1\n",
      "1267  2022-01-01  21:28:39  192.151.139.83        401       1\n",
      "1268  2022-01-01  21:28:41  192.151.139.83        401       1\n",
      "1269  2022-01-01  21:28:44  192.151.139.83        401       1\n",
      "1270  2022-01-01  21:28:47  192.151.139.83        401       1\n",
      "1271  2022-01-01  21:28:49  192.151.139.83        401       1\n",
      "1272  2022-01-01  21:28:50  192.151.139.83        401       1\n",
      "1273  2022-01-01  21:28:53  192.151.139.83        401       1\n",
      "1274  2022-01-01  21:28:55  192.151.139.83        401       1\n",
      "1275  2022-01-01  21:28:57  192.151.139.83        401       1\n",
      "1276  2022-01-01  21:28:59  192.151.139.83        401       1\n",
      "1277  2022-01-01  21:29:01  192.151.139.83        401       1\n",
      "1278  2022-01-01  21:29:02  192.151.139.83        401       1\n",
      "1279  2022-01-01  21:29:05  192.151.139.83        401       1\n",
      "1280  2022-01-01  21:29:06  192.151.139.83        401       1\n",
      "1283  2022-01-01  21:29:09  192.151.139.83        401       1\n",
      "1284  2022-01-01  21:29:12  192.151.139.83        401       1\n",
      "1285  2022-01-01  21:29:13  192.151.139.83        401       1\n",
      "1286  2022-01-01  21:29:16  192.151.139.83        401       1\n",
      "1287  2022-01-01  21:29:18  192.151.139.83        401       1\n",
      "1288  2022-01-01  21:29:20  192.151.139.83        401       1\n",
      "1289  2022-01-01  21:29:21  192.151.139.83        401       1\n",
      "1290  2022-01-01  21:29:24  192.151.139.83        401       1\n",
      "1291  2022-01-01  21:29:27  192.151.139.83        401       1\n",
      "1292  2022-01-01  21:29:28  192.151.139.83        401       1\n",
      "1293  2022-01-01  21:29:29  192.151.139.83        401       1\n",
      "1294  2022-01-01  21:29:30  192.151.139.83        401       1\n",
      "1295  2022-01-01  21:29:31  192.151.139.83        401       1\n",
      "1297  2022-01-01  21:29:34  192.151.139.83        401       1\n",
      "1298  2022-01-01  21:29:35  192.151.139.83        401       1\n",
      "1299  2022-01-01  21:29:36  192.151.139.83        401       1\n",
      "1301  2022-01-01  21:29:39  192.151.139.83        401       1\n",
      "1302  2022-01-01  21:29:40  192.151.139.83        401       1\n",
      "1303  2022-01-01  21:29:42  192.151.139.83        401       1\n",
      "1304  2022-01-01  21:29:44  192.151.139.83        401       1\n",
      "1305  2022-01-01  21:29:45  192.151.139.83        401       1\n",
      "1306  2022-01-01  21:29:47  192.151.139.83        401       1\n",
      "1307  2022-01-01  21:29:50  192.151.139.83        401       1\n",
      "1308  2022-01-01  21:29:52  192.151.139.83        401       1\n",
      "1309  2022-01-01  21:29:54  192.151.139.83        401       1\n",
      "1310  2022-01-01  21:29:56  192.151.139.83        401       1\n",
      "1311  2022-01-01  21:29:58  192.151.139.83        401       1\n",
      "1312  2022-01-01  21:30:00  192.151.139.83        401       1\n",
      "1313  2022-01-01  21:30:01  192.151.139.83        401       1\n",
      "1314  2022-01-01  21:30:03  192.151.139.83        401       1\n",
      "1315  2022-01-01  21:30:04  192.151.139.83        401       1\n",
      "1316  2022-01-01  21:30:05  192.151.139.83        401       1\n",
      "1317  2022-01-01  21:30:06  192.151.139.83        401       1\n",
      "1318  2022-01-01  21:30:09  192.151.139.83        401       1\n",
      "1319  2022-01-01  21:30:12  192.151.139.83        401       1\n",
      "1320  2022-01-01  21:30:15  192.151.139.83        401       1\n",
      "1321  2022-01-01  21:30:16  192.151.139.83        401       1\n",
      "1322  2022-01-01  21:30:17  192.151.139.83        401       1\n",
      "1323  2022-01-01  21:30:18  192.151.139.83        401       1\n",
      "1324  2022-01-01  21:30:21  192.151.139.83        401       1\n",
      "1325  2022-01-01  21:30:24  192.151.139.83        401       1\n",
      "1326  2022-01-01  21:30:27  192.151.139.83        401       1\n",
      "1327  2022-01-01  21:30:28  192.151.139.83        401       1\n",
      "1328  2022-01-01  21:30:30  192.151.139.83        401       1\n",
      "1329  2022-01-01  21:30:33  192.151.139.83        401       1\n",
      "1330  2022-01-01  21:30:35  192.151.139.83        401       1\n",
      "1331  2022-01-01  21:30:38  192.151.139.83        401       1\n",
      "1332  2022-01-01  21:30:41  192.151.139.83        401       1\n",
      "1333  2022-01-01  21:30:43  192.151.139.83        401       1\n",
      "1334  2022-01-01  21:30:46  192.151.139.83        401       1\n",
      "1335  2022-01-01  21:30:48  192.151.139.83        401       1\n",
      "1336  2022-01-01  21:30:51  192.151.139.83        401       1\n",
      "1337  2022-01-01  21:30:54  192.151.139.83        401       1\n",
      "1338  2022-01-01  21:30:55  192.151.139.83        401       1\n",
      "1339  2022-01-01  21:30:58  192.151.139.83        401       1\n",
      "1340  2022-01-01  21:31:00  192.151.139.83        401       1\n",
      "1341  2022-01-01  21:31:03  192.151.139.83        401       1\n",
      "1342  2022-01-01  21:31:04  192.151.139.83        401       1\n",
      "1343  2022-01-01  21:31:07  192.151.139.83        401       1\n",
      "1344  2022-01-01  21:31:09  192.151.139.83        401       1\n",
      "1345  2022-01-01  21:31:10  192.151.139.83        401       1\n",
      "1346  2022-01-01  21:31:12  192.151.139.83        401       1\n",
      "1347  2022-01-01  21:31:13  192.151.139.83        401       1\n",
      "1348  2022-01-01  21:31:15  192.151.139.83        401       1\n",
      "1349  2022-01-01  21:31:17  192.151.139.83        401       1\n",
      "1350  2022-01-01  21:31:19  192.151.139.83        401       1\n",
      "1351  2022-01-01  21:31:21  192.151.139.83        401       1\n",
      "1352  2022-01-01  21:31:22  192.151.139.83        401       1\n",
      "1353  2022-01-01  21:31:25  192.151.139.83        401       1\n",
      "1354  2022-01-01  21:31:28  192.151.139.83        401       1\n",
      "1355  2022-01-01  21:31:29  192.151.139.83        401       1\n",
      "1356  2022-01-01  21:31:31  192.151.139.83        401       1\n",
      "1357  2022-01-01  21:31:34  192.151.139.83        401       1\n",
      "1358  2022-01-01  21:31:35  192.151.139.83        401       1\n",
      "1359  2022-01-01  21:31:37  192.151.139.83        401       1\n",
      "1360  2022-01-01  21:31:40  192.151.139.83        401       1\n",
      "1361  2022-01-01  21:31:42  192.151.139.83        401       1\n",
      "1362  2022-01-01  21:31:44  192.151.139.83        401       1\n",
      "1363  2022-01-01  21:31:46  192.151.139.83        401       1\n",
      "1364  2022-01-01  21:31:48  192.151.139.83        401       1\n",
      "1365  2022-01-01  21:31:51  192.151.139.83        401       1\n",
      "1366  2022-01-01  21:31:53  192.151.139.83        401       1\n",
      "1367  2022-01-01  21:31:54  192.151.139.83        401       1\n",
      "1368  2022-01-01  21:31:56  192.151.139.83        401       1\n",
      "1369  2022-01-01  21:31:58  192.151.139.83        401       1\n",
      "1370  2022-01-01  21:31:59  192.151.139.83        401       1\n",
      "1371  2022-01-01  21:32:02  192.151.139.83        401       1\n",
      "1372  2022-01-01  21:32:03  192.151.139.83        401       1\n",
      "1373  2022-01-01  21:32:06  192.151.139.83        401       1\n",
      "1374  2022-01-01  21:32:08  192.151.139.83        401       1\n",
      "1375  2022-01-01  21:32:10  192.151.139.83        401       1\n",
      "1376  2022-01-01  21:32:13  192.151.139.83        401       1\n",
      "1377  2022-01-01  21:32:15  192.151.139.83        401       1\n"
     ]
    }
   ],
   "source": [
    "pd.set_option('display.max_rows', None)\n",
    "time_frame_group=data.groupby(['date','time','c-ip', 'sc-status']).size()\n",
    "\n",
    "count_time_frame=time_frame_group.to_frame(name=\"counts\").reset_index()\n",
    "\n",
    "sip2=(count_time_frame['sc-status']==401)&(count_time_frame['c-ip']=='192.151.139.83')\n",
    "\n",
    "print(count_time_frame[sip2])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "This summary shows a rundown of the unauthorized attempts from the ip adress **192.151.139.83** in relation to the time that the attempts were made. <br>\n",
    "The attempts were made on the same day with an average of 1-2 seconds time difference between each attempts "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 94,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "             date      time           c-ip  sc-status  counts\n",
      "26389  2022-01-24  09:57:05  195.12.35.175        401       1\n",
      "26390  2022-01-24  09:57:06  195.12.35.175        401       1\n",
      "26391  2022-01-24  09:57:07  195.12.35.175        401       1\n",
      "26392  2022-01-24  09:57:08  195.12.35.175        401       1\n",
      "26393  2022-01-24  09:57:09  195.12.35.175        401       1\n",
      "26394  2022-01-24  09:57:10  195.12.35.175        401       1\n",
      "26395  2022-01-24  09:57:13  195.12.35.175        401       1\n",
      "26396  2022-01-24  09:57:15  195.12.35.175        401       1\n",
      "26399  2022-01-24  09:57:17  195.12.35.175        401       1\n",
      "26400  2022-01-24  09:57:18  195.12.35.175        401       1\n",
      "26401  2022-01-24  09:57:19  195.12.35.175        401       1\n",
      "26402  2022-01-24  09:57:20  195.12.35.175        401       1\n",
      "26403  2022-01-24  09:57:23  195.12.35.175        401       1\n",
      "26404  2022-01-24  09:57:26  195.12.35.175        401       1\n",
      "26405  2022-01-24  09:57:29  195.12.35.175        401       1\n",
      "26406  2022-01-24  09:57:31  195.12.35.175        401       1\n",
      "26407  2022-01-24  09:57:32  195.12.35.175        401       1\n",
      "26408  2022-01-24  09:57:34  195.12.35.175        401       1\n",
      "26409  2022-01-24  09:57:37  195.12.35.175        401       1\n",
      "26410  2022-01-24  09:57:39  195.12.35.175        401       1\n",
      "26411  2022-01-24  09:57:41  195.12.35.175        401       1\n",
      "26412  2022-01-24  09:57:42  195.12.35.175        401       1\n",
      "26414  2022-01-24  09:57:43  195.12.35.175        401       1\n",
      "26415  2022-01-24  09:57:46  195.12.35.175        401       1\n",
      "26416  2022-01-24  09:57:48  195.12.35.175        401       1\n",
      "26417  2022-01-24  09:57:49  195.12.35.175        401       1\n",
      "26420  2022-01-24  09:57:51  195.12.35.175        401       1\n",
      "26421  2022-01-24  09:57:54  195.12.35.175        401       1\n",
      "26422  2022-01-24  09:57:56  195.12.35.175        401       1\n",
      "26423  2022-01-24  09:57:58  195.12.35.175        401       1\n",
      "26424  2022-01-24  09:58:01  195.12.35.175        401       1\n",
      "26425  2022-01-24  09:58:02  195.12.35.175        401       1\n",
      "26426  2022-01-24  09:58:04  195.12.35.175        401       1\n",
      "26427  2022-01-24  09:58:06  195.12.35.175        401       1\n",
      "26429  2022-01-24  09:58:08  195.12.35.175        401       1\n",
      "26430  2022-01-24  09:58:11  195.12.35.175        401       1\n",
      "26431  2022-01-24  09:58:12  195.12.35.175        401       1\n",
      "26433  2022-01-24  09:58:14  195.12.35.175        401       1\n",
      "26434  2022-01-24  09:58:15  195.12.35.175        401       1\n",
      "26435  2022-01-24  09:58:16  195.12.35.175        401       1\n",
      "26437  2022-01-24  09:58:19  195.12.35.175        401       1\n",
      "26438  2022-01-24  09:58:21  195.12.35.175        401       1\n",
      "26439  2022-01-24  09:58:24  195.12.35.175        401       1\n",
      "26440  2022-01-24  09:58:26  195.12.35.175        401       1\n",
      "26441  2022-01-24  09:58:28  195.12.35.175        401       1\n",
      "26442  2022-01-24  09:58:29  195.12.35.175        401       1\n",
      "26443  2022-01-24  09:58:30  195.12.35.175        401       1\n",
      "26444  2022-01-24  09:58:31  195.12.35.175        401       1\n",
      "26445  2022-01-24  09:58:32  195.12.35.175        401       1\n",
      "26446  2022-01-24  09:58:35  195.12.35.175        401       1\n",
      "26447  2022-01-24  09:58:36  195.12.35.175        401       1\n",
      "26448  2022-01-24  09:58:37  195.12.35.175        401       1\n",
      "26449  2022-01-24  09:58:40  195.12.35.175        401       1\n",
      "26450  2022-01-24  09:58:42  195.12.35.175        401       1\n",
      "26451  2022-01-24  09:58:45  195.12.35.175        401       1\n",
      "26452  2022-01-24  09:58:46  195.12.35.175        401       1\n",
      "26453  2022-01-24  09:58:48  195.12.35.175        401       1\n",
      "26454  2022-01-24  09:58:49  195.12.35.175        401       1\n",
      "26455  2022-01-24  09:58:52  195.12.35.175        401       1\n",
      "26456  2022-01-24  09:58:54  195.12.35.175        401       1\n",
      "26457  2022-01-24  09:58:56  195.12.35.175        401       1\n",
      "26458  2022-01-24  09:58:59  195.12.35.175        401       1\n",
      "26459  2022-01-24  09:59:02  195.12.35.175        401       1\n",
      "26460  2022-01-24  09:59:03  195.12.35.175        401       1\n",
      "26461  2022-01-24  09:59:05  195.12.35.175        401       1\n",
      "26462  2022-01-24  09:59:06  195.12.35.175        401       1\n",
      "26463  2022-01-24  09:59:08  195.12.35.175        401       1\n",
      "26464  2022-01-24  09:59:10  195.12.35.175        401       1\n",
      "26465  2022-01-24  09:59:11  195.12.35.175        401       1\n",
      "26466  2022-01-24  09:59:14  195.12.35.175        401       1\n",
      "26467  2022-01-24  09:59:17  195.12.35.175        401       1\n",
      "26468  2022-01-24  09:59:18  195.12.35.175        401       1\n",
      "26469  2022-01-24  09:59:21  195.12.35.175        401       1\n",
      "26470  2022-01-24  09:59:24  195.12.35.175        401       1\n",
      "26471  2022-01-24  09:59:27  195.12.35.175        401       1\n",
      "26472  2022-01-24  09:59:28  195.12.35.175        401       1\n",
      "26473  2022-01-24  09:59:31  195.12.35.175        401       1\n",
      "26474  2022-01-24  09:59:32  195.12.35.175        401       1\n",
      "26475  2022-01-24  09:59:34  195.12.35.175        401       1\n",
      "26476  2022-01-24  09:59:36  195.12.35.175        401       1\n",
      "26477  2022-01-24  09:59:38  195.12.35.175        401       1\n",
      "26478  2022-01-24  09:59:40  195.12.35.175        401       1\n",
      "26479  2022-01-24  09:59:41  195.12.35.175        401       1\n",
      "26480  2022-01-24  09:59:42  195.12.35.175        401       1\n",
      "26481  2022-01-24  09:59:44  195.12.35.175        401       1\n",
      "26482  2022-01-24  09:59:45  195.12.35.175        401       1\n",
      "26483  2022-01-24  09:59:47  195.12.35.175        401       1\n",
      "26484  2022-01-24  09:59:50  195.12.35.175        401       1\n",
      "26485  2022-01-24  09:59:53  195.12.35.175        401       1\n",
      "26486  2022-01-24  09:59:56  195.12.35.175        401       1\n",
      "26487  2022-01-24  09:59:58  195.12.35.175        401       1\n",
      "26488  2022-01-24  09:59:59  195.12.35.175        401       1\n",
      "26489  2022-01-24  10:00:02  195.12.35.175        401       1\n",
      "26490  2022-01-24  10:00:04  195.12.35.175        401       1\n",
      "26491  2022-01-24  10:00:06  195.12.35.175        401       1\n",
      "26492  2022-01-24  10:00:07  195.12.35.175        401       1\n",
      "26493  2022-01-24  10:00:10  195.12.35.175        401       1\n",
      "26494  2022-01-24  10:00:13  195.12.35.175        401       1\n",
      "26495  2022-01-24  10:00:14  195.12.35.175        401       1\n",
      "26496  2022-01-24  10:00:16  195.12.35.175        401       1\n",
      "26497  2022-01-24  10:00:18  195.12.35.175        401       1\n",
      "26498  2022-01-24  10:00:19  195.12.35.175        401       1\n",
      "26499  2022-01-24  10:00:20  195.12.35.175        401       1\n",
      "26500  2022-01-24  10:00:23  195.12.35.175        401       1\n",
      "26501  2022-01-24  10:00:26  195.12.35.175        401       1\n",
      "26502  2022-01-24  10:00:28  195.12.35.175        401       1\n",
      "26503  2022-01-24  10:00:29  195.12.35.175        401       1\n",
      "26504  2022-01-24  10:00:32  195.12.35.175        401       1\n",
      "26505  2022-01-24  10:00:34  195.12.35.175        401       1\n",
      "26506  2022-01-24  10:00:37  195.12.35.175        401       1\n",
      "26507  2022-01-24  10:00:39  195.12.35.175        401       1\n",
      "26508  2022-01-24  10:00:41  195.12.35.175        401       1\n",
      "26509  2022-01-24  10:00:42  195.12.35.175        401       1\n",
      "26510  2022-01-24  10:00:44  195.12.35.175        401       1\n",
      "26511  2022-01-24  10:00:47  195.12.35.175        401       1\n",
      "26512  2022-01-24  10:00:49  195.12.35.175        401       1\n",
      "26513  2022-01-24  10:00:52  195.12.35.175        401       1\n",
      "26514  2022-01-24  10:00:54  195.12.35.175        401       1\n",
      "26515  2022-01-24  10:00:57  195.12.35.175        401       1\n",
      "26518  2022-01-24  10:01:00  195.12.35.175        401       1\n",
      "26519  2022-01-24  10:01:02  195.12.35.175        401       1\n",
      "26520  2022-01-24  10:01:04  195.12.35.175        401       1\n",
      "26523  2022-01-24  10:01:07  195.12.35.175        401       1\n"
     ]
    }
   ],
   "source": [
    "pd.set_option('display.max_rows', None)\n",
    "time_frame_group=data.groupby(['date','time','c-ip', 'sc-status']).size()\n",
    "\n",
    "count_time_frame=time_frame_group.to_frame(name=\"counts\").reset_index()\n",
    "\n",
    "sip3=(count_time_frame['sc-status']==401)&(count_time_frame['c-ip']=='195.12.35.175')\n",
    "\n",
    "print(count_time_frame[sip3])"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "This summary shows a rundown of the unauthorized attempts from the ip adress **195.12.35.175** in relation to the time that the attempts were made. <br>\n",
    "The attempts were made on the same day with an average of 1-2 seconds time difference between each attempts "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "               c-ip  sc-status cs-method  cs-uri-stem  counts\n",
      "3251  149.5.250.189        301       GET   index.aspx       1\n",
      "3252  149.5.250.189        301       GET  logout.aspx       1\n",
      "3253  149.5.250.189        301      POST   login.aspx       1 \n",
      "\n",
      "                 c-ip  sc-status cs-method  cs-uri-stem  counts\n",
      "11977  192.151.139.83        301       GET   index.aspx       1\n",
      "11978  192.151.139.83        301       GET  logout.aspx       1\n",
      "11979  192.151.139.83        301      POST   login.aspx       1 \n",
      "\n",
      "                c-ip  sc-status cs-method  cs-uri-stem  counts\n",
      "16446  195.12.35.175        301       GET   index.aspx       1\n",
      "16447  195.12.35.175        301       GET  logout.aspx       1\n",
      "16448  195.12.35.175        301      POST   login.aspx       1 \n",
      "\n"
     ]
    }
   ],
   "source": [
    "method_frame_group=data.groupby(['c-ip', 'sc-status','cs-method','cs-uri-stem']).size()\n",
    "count_method=method_frame_group.to_frame(name=\"counts\").reset_index()\n",
    "\n",
    "count_301=(count_method['sc-status']==301)&(count_method['c-ip']=='149.5.250.189')\n",
    "print(count_method[count_301] ,\"\\n\")\n",
    "\n",
    "\n",
    "\n",
    "count_301=(count_method['sc-status']==301)&(count_method['c-ip']=='192.151.139.83')\n",
    "print(count_method[count_301] ,\"\\n\")\n",
    "\n",
    "\n",
    "count_301=(count_method['sc-status']==301)&(count_method['c-ip']=='195.12.35.175')\n",
    "print(count_method[count_301] ,\"\\n\")"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 301\n",
    "The summary above is to check if there were any successful login from the suspected IP addresses, it shows that each of the IP addresses had at least one successful login with the POST method, as a redirection (301) to the index and logout page are present"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "             c-ip                                     cs(User-Agent)  counts\n",
      "88  149.5.250.189  Mozilla/5.0+(iPhone;+CPU+iPhone+OS+9_0+like+Ma...     287 \n",
      "\n",
      "               c-ip                                     cs(User-Agent)  counts\n",
      "340  192.151.139.83  Mozilla/5.0+(iPhone;+CPU+iPhone+OS+9_0+like+Ma...     215 \n",
      "\n",
      "              c-ip                                     cs(User-Agent)  counts\n",
      "468  195.12.35.175  Mozilla/5.0+(iPhone;+CPU+iPhone+OS+9_0+like+Ma...     201 \n",
      "\n"
     ]
    }
   ],
   "source": [
    "useragent_frame_group=data.groupby(['c-ip','cs(User-Agent)']).size()\n",
    "count_uagent=useragent_frame_group.to_frame(name=\"counts\").reset_index()\n",
    "count_uagent_f=(count_uagent['c-ip']=='149.5.250.189')\n",
    "print(count_uagent[count_uagent_f],\"\\n\")\n",
    "\n",
    "useragent_frame_group=data.groupby(['c-ip','cs(User-Agent)']).size()\n",
    "count_uagent=useragent_frame_group.to_frame(name=\"counts\").reset_index()\n",
    "count_uagent_f=(count_uagent['c-ip']=='192.151.139.83')\n",
    "print(count_uagent[count_uagent_f],\"\\n\")\n",
    "\n",
    "\n",
    "useragent_frame_group=data.groupby(['c-ip','cs(User-Agent)']).size()\n",
    "count_uagent=useragent_frame_group.to_frame(name=\"counts\").reset_index()\n",
    "count_uagent_f=(count_uagent['c-ip']=='195.12.35.175')\n",
    "print(count_uagent[count_uagent_f],\"\\n\")\n",
    "\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The above summary indicates that similar device type and browser were used to make the attempts, it might imply that the attempts were made by the same person or group of people. <br> This however could be fake data intentionally sent by the attacker."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# My Conclusion"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "After investigation of the system logs, the following IP addresses appear to be malicious:\n",
    "1. 149.5.250.189        \n",
    "2. 192.151.139.83        \n",
    "3. 195.12.35.175 \n",
    "\n",
    "The activity in the logs shows a bruteforce attack as there are multiple unuathorized attempts [401](#401)and at least one successful login [301](#301).\n",
    "\n",
    "Something else to note is that the attempts were made on different days\n",
    "<br>\n",
    "Further investigation below can be carried out to further drill down on the attack\n",
    "1. Is the traffic coming from a recognised host username to determine is user's login credentials has been compromised.\n",
    "2. Are the requests coming from a recognised device to dtermine if a user's device has been compromised"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# References\n",
    "\n",
    "pandas.DataFrame.groupby Available from: __[Pandas Documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.groupby.html)__ [Accessed 01 March 2022]"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3 (ipykernel)",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.13.2"
  },
  "widgets": {
   "application/vnd.jupyter.widget-state+json": {
    "state": {},
    "version_major": 2,
    "version_minor": 0
   }
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}
