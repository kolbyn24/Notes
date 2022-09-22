---
creation date: May 26th 2022
last modified date: May 26th 2022
aliases: []
tags: #📖
---

Primary Categories: [[01 - Operation]]
Secondary Categories:  [[02 - Internal]]
Links: 
Search Tag: #📖  

# [[Printer Credentials]]  
Finding credentials in printers can be assisted with the following script. Run this against the PeepingTom file or similar to discover potential printers with no administrator password set. 
```python
# https://IP/hp/device/info_config_network.html?tab=Networking&menu=NetConfig

import requests
from bs4 import BeautifulSoup
import sys 
import urllib3
urllib3.disable_warnings()


printer_map = {
	'HP LaserJet': '/hp/device/info_config_network.html?tab=Networking&menu=NetConfig',
	'HP Color LaserJet': '/hp/device/info_config_network.html?tab=Networking&menu=NetConfig',
	'LaserJet': 'None',
	'Printer': 'None'
}


def get_url_for_host(host):
	result = None

	try:
		resp = requests.get(f'{host}/hp/device/info_config_network.html?tab=Networking&menu=NetConfig', verify=False, timeout=10)

		for k in printer_map.keys():
			if k in resp.text:
				result = printer_map[k]
				break
	except Exception as e:
		 print(e)
	finally:
		return result


def main():
	with open(sys.argv[1], 'r') as f:
		for host in f.readlines():
			try:
				host = host.rstrip('\n')

				url = get_url_for_host(host)
				if url == 'None':
					print(f'[-] {host}|This printer requires manual investigation.')
				elif url is None:
					print(f'[-] {host}|This device is not supported.')
				else:
					resp = requests.get(f'{host}{url}', verify=False, timeout=10)
					page = resp.content

					soup = BeautifulSoup(page, "html.parser")
					labels = soup.find_all("td", attrs={"class": "labelFont"})

					for label in labels:
						if label.text.strip() == "Administrator Password:":
							parent = label.parent
							setting = parent.find("td", attrs={"class": "itemFont"})
							if setting.text.strip() == "Not Specified":
								print(f'[+] {host}|{resp.status_code} - Password not required!')
							elif setting.text.strip() == "Specified":
								print(f'[-] {host}|{resp.status_code} - Password required...')
							else:
								print(f'[!] {host}|{resp.status_code} - ({setting.text.strip()}) Manual investigation required...')

							break
				

			except Exception as e:
				print(f'{host}|{e}')

if __name__ == "__main__":
	main()
```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: May 26th 2022 (02:31 pm)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
