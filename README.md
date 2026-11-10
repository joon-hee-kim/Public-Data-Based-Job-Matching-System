# Public Data-Based Job Matching System
This is the result of a project that utilized and applied public data related to childcare centers and labor (job opening, job search) to machine learning algorithms.

In this project, "childcare facilities" and "childcare centers" (named `childcare` in the code) mean child welfare institutions (보육원) for children without parental care, not daycare centers. "Orphanage leavers" means youth leaving these institutions.

This project was also submitted to the "2024 National Happiness Service Discovery and Entrepreneurship Contest," organized by the Korea Social Security Information Service, where it received a Selected Entry award.

[Contest Link](https://www.ssis.or.kr/lay1/bbs/S1T67C95/A/102/view.do?article_seq=125799&cpage=1&rows=10&condition=&keyword=)

## Motivation
In a situation where orphanage leavers and those in need of protection continue to experience economic difficulties, and in South Korea, where labor shortages are worsening as we enter a super-aged society, the introduction of a job matching system for orphanage leavers is an urgent task.

## Output
**Step 1. Data Curation** (Reference to **Data Source**)</br></br>
**Step 2. Data Preprocessing** (Reference to **Algorithm**)</br></br>
**Step 3. Modeling** </br>

- #1. Perform regional latitude/longitude-based clustering (100 clusters) using the K-Means algorithm </br>

  <img width="452" alt="그림1" src="https://github.com/joon-hee-kim/Public-Data-Based-Job-Matching-System/assets/121689436/e458c6df-a820-48ad-9f52-195c60e54d0b"> </br>


- #2. Using the K-Means algorithm again, perform a second clustering (50 clusters) based on the results of #1 and the 2022 labor demand ratio </br>

  <img width="452" alt="그림2" src="https://github.com/joon-hee-kim/Public-Data-Based-Job-Matching-System/assets/121689436/548f621a-0517-4d71-92f3-43bf14cc9df1"> </br>


**Step 4. Analysis and visualization** </br>

- The heatmap shows the center of each cluster, weighted by its average labor demand ratio. It was designed so that red indicates high labor demand (a low ratio) and green indicates low labor demand (a high ratio). </br>
- Therefore, population should be allocated first to regions with a lower ratio, following the ranking in Step 5. </br>

  <img width="452" alt="그림3" src="https://github.com/joon-hee-kim/Public-Data-Based-Job-Matching-System/assets/121689436/c426552d-7d74-4470-acc6-2920c49f1822"> </br>


**Step 5. Childcare facility matching**</br>

- Distance calculation </br>
- The figure below shows the step for calculating the distance between childcare facilities and labor demand areas. </br>

  <img width="452" alt="그림4" src="https://github.com/joon-hee-kim/Public-Data-Based-Job-Matching-System/assets/121689436/f06ae4bf-b01b-49f9-9dd0-64695f602ed1"> </br>


- Ranking calculation </br>

  | 노동수치_군집 | 배치 |
  |:---:|:---:|
  | 0 | 27 |
  | 1 | 10 |
  | 2 | 24 |
  | 3 | 17 |
  | 4 | 28 |
  | … | … |
  | 45 | 12 |
  | 46 | 45 |
  | 47 | 26 |
  | 48 | 20 |
  | 49 | 4 |

- This table ranks the clusters by their average labor demand ratio, while distance is used separately in the matching step to find the nearest childcare facilities. A lower ratio means a more severe labor shortage, so the cluster gets a higher priority for employment placement. </br>
- The value in the “노동수치_군집” column is the ID of each cluster, and the value in the “배치” column is its priority rank, starting from 0 (0 = highest priority). For example, Cluster 0 has a value of 27, so it is 28th out of 50 clusters. </br>



**Final result** </br>
- As an example, the figure below shows the three childcare centers closest to the center of cluster 49. </br>
(“노동수치 군집 번호”, “보육 시설”, and “거리” mean the labor cluster ID, the childcare center, and the distance in km.) </br>

  <img width="549" alt="스크린샷 2024-06-16 오후 11 46 19" src="https://github.com/joon-hee-kim/Public-Data-Based-Job-Matching-System/assets/121689436/2b81761b-e5da-42cd-b476-31cf0dd4408a"> </br></br>


## Algorithm
<img width="795" alt="스크린샷 2024-06-15 오후 4 55 54" src="https://github.com/joon-hee-kim/Public-Data-Based-Job-Matching-System/assets/121689436/958aed18-6dd0-4165-a482-7b43f6b9d353"> </br>

**K-Means clustering** was used for regional grouping. For region–childcare matching, geographic distance was calculated to identify the nearest childcare facilities. Data preprocessing was conducted using regional job opening and job search data.

- Labor Demand Ratio: The number of job searches divided by the number of job openings in each region. Four yearly ratios (2019–2022) were calculated, and the 2022 ratio was used in the model. A lower value indicates a more severe labor shortage. <br>
- Required Workforce: A derived indicator equal to the total number of job openings minus the total number of job searches over 2019–2022. A positive value means there are more openings than job seekers. <br>
- After removing records with missing values or missing coordinates, only regions with a 2022 ratio of 1 or less and a non-negative required workforce were used (121 of the 264 labor-market records).

## 👥 Team Member

201934219 Kim Joonhee </br>

202031503 Kang Jiwon </br>

202034305 Kim Dabin </br>

202031512 Kim Minjae </br>

 
## ✔️ Data Source
Childcare Facility Data
* Status of child welfare facilities in 2022.  (Ministry of Health and Welfare): [Data Link](https://www.mohw.go.kr/board.es?mid=a10412000000&bid=0020&act=view&list_no=377895&tag=&nPage=1) </br>
* List of Child Welfare Facilities in Seoul (Source: Seoul Open Data Plaza): [Data Link](https://data.seoul.go.kr/dataList/OA-20420/S/1/datasetView.do)
* Status of Child Care Facilities (Source: Gyeonggi Data Dream): [Data Link](https://data.gg.go.kr/portal/data/service/selectServicePage.do?page=1&rows=10&sortColumn=&sortDirection=&infId=VM9U79G9GCT38059YX9M12361361&infSeq=1&order=&loc=&searchWord=%EC%95%84%EB%8F%99+%EC%96%91%EC%9C%A1%EC%8B%9C%EC%84%A4+%ED%98%84%ED%99%A9)
* Busan Child Care Facilities (Source: Public Data Portal): [Data Link](https://www.data.go.kr/data/3076473/openapi.do)
* Daegu Child Welfare Facilities (Source: Daegu Metropolitan City Website): [Data Link](https://www.daegu.go.kr/woman/index.do;jsessionid=FACDD17BA231BF2BA14904508E7A6FC1.tomcat2?menu_id=00050130&servletPath=%2Fwoman)
* Incheon Metropolitan City_Child Welfare Facilities (Statistics)_20221231 (Source: Public Data Portal): [Data Link](https://www.data.go.kr/data/15064922/fileData.do)
* Gwangju Metropolitan City_Child Care Facilities Status_20231124 (Source: Public Data Portal): [Data Link](https://www.data.go.kr/data/15043858/fileData.do)
* Daejeon Child Care Facilities (Source: Daejeon Metropolitan City Child Association of Child Welfare): [Data Link](http://www.djchild.or.kr/page/facilities.php?device=pc)
* Sejong Child Welfare Facilities Status (As of April 2023) (Source: Sejong City): [Data Link](https://www.sejong.go.kr/bbs/R0595/view.do?nttId=B000000089056Kp1tB1f&mno=sub01_0101&cmsNoStr=&kind=&pageIndex=1)
* Chungcheongbuk-do Child Welfare Facilities (Source: Chungcheongbuk Province Government): [Data Link](https://www.chungbuk.go.kr/www/contents.do?key=4793)
* Chungcheongnam-do Child Care Facilities (Source: adongbokji): [Data Link](http://www.adongbokji.or.kr/adong/m/html/2_1.asp?page=2&address_1=%C3%E6%B3%B2&gubun2=&keyword1=&PB=%C8%B8%BF%F8%BD%C3%BC%B3%B0%CB%BB%F6#gotop)
* Jeollanam-do Child Care Facilities (Source: Jeonnam Association of Child Welfare): [Data Link](http://jn-adong.co.kr/bbs/sub_page_02.php)
* Gyeongsangbuk-do_Child Care Facilities Status_20231208 (Source: Public Data Portal): [Data Link](https://www.data.go.kr/data/15063057/fileData.do)
* Gyeongsangnam-do_Child Care Facilities_20221231 (Source: Public Data Portal): [Data Link](https://www.data.go.kr/data/15068287/fileData.do)
* Gangwon-do Child Care Facilities (Source: Korea Child Welfare Facilities): [Data Link](http://www.adongbokji.or.kr/adong/m/html/2_1.asp?PB=%C8%B8%BF%F8%BD%C3%BC%B3%B0%CB%BB%F6&address_1=%B0%AD%BF%F8#gotop)
* Jeju Special Self-Governing Province_Child Care Facilities Status_20230803 (Source: Public Data Portal): [Data Link](https://www.data.go.kr/data/3083487/fileData.do)
* Jeollabuk-do Child Care Facilities (Source: adongbokji): [Data Link](http://www.adongbokji.or.kr/adong/m/html/2_1.asp?page=2&address_1=%C0%FC%BA%CF&gubun2=&keyword1=&PB=%C8%B8%BF%F8%BD%C3%BC%B3%B0%CB%BB%F6#gotop)
* Number of Children in Need by Gender and Placement (202201~202212) (Source: Bokjiro; background reference): [Data Link](https://www.bokjiro.go.kr/ssis-tbu/twatga/sociGuaStat/SociGuaStatDetailIframe.do?datsNo=45&datsClNo=1051&datsClCrit=WS)

Labor-related Data
* Labor Market Status_Job Openings and Job Searches_Job Openings, Job Searches, Placements, and Employment Status (Source: Korea Employment Information Service) [Data Link](https://www.data.go.kr/data/15067938/fileData.do?recommendDataYn=Y) </br>


