# RMF Systems Engineering Handbook

1st July 2022

## What is the RMF@TRL project about?
In the course of building the [RMF](https://osrf.github.io/ros2multirobotbook/intro.html), the team observed many questions being asked 
(by Government agencies, end users and developers), about RMF's 
- Safety & reliability
- Operational concerns 
- Policy and regulations.

To address these concerns, Singapore's [S&TPPO](https://www.sgdi.gov.sg/ministries/pmo/departments/stppo) and [SNDGG](https://www.smartnation.gov.sg/about-smart-nation/sndgg)
have put together a pilot project centered on [National Library Board's](https://www.nlb.gov.sg/) premises at 
[Tampines Regional](https://www.nlb.gov.sg/VisitUs/BranchDetails/tabid/140/bid/335/Default.aspx?branch=Tampines+Regional+Library)

The project team (in collaboration with OSRF Singapore) will be taking a *test centric* approach in our development & deployment of RMF at TRL, to collect real world data to evaluate these issues.


## Motivations for RMF Systems Engineering Handbook
At higher levels of consideration (from a systems engineering standpoint), we note the following trends for ROS usage:
- ROS1 has been massively successful as an open source effort amongst the academic community
	- Promoting reuse of robotics developments across students, researchers and developers
	- High rates of adoption across robotics community, particularly in promoting the ROS standard message libraries for interoperability
- ROS2 has been steadily gaining traction with automotive components suppliers ie: [Apex.AI](https://www.apex.ai/)
	- Noted ROS1 has reached end of life (noetic)
	- ROS2 is supposed to champion *both* open source efforts *and* commercially ready packages (ie: ROS2 RMWs)

Key outcome we want to arrive at, is a *operational* (safe, functional and reliable) RMF system. 
Thus to bridge the divide from ROS1/ROS2 towards an operational outcome, is not just "software" issues.
There are orthogonal considerations, (ie: systems engineering concerns), that need to be addressed:
- Risk assessments and hazards analysis
- Quality assurance & quality control issues (ie: Patform hardware and software, operational safety)
- Platform safety and operational standards
- Design documentations and traceability
- Qualifications plans and system test/comissioning plans


>"The camel, was a horse, but designed by a committee..." <br> - Sir Alec Issigonis, designer of the Morris Mini, 1959

Turns out these are rather contentious issues.
After several months of study, the project team has outlined an approach that we think strikes the best balance of the following factors:
- Cost
- Complexity
- Performance
- Interoperability

> Insert diagram of "Platform -> Qualifications -> Operations" here

For this to work however, we will be sharing both our approach (in this guide book) and the data that we have collected, for peer review process. Thus we need the 

Do email us your questions and observations, that we can refine this *live* systems engineering process, so that we can create a new kind of cyber-physical network where
robots can roam freely across interlinked premises!

Thanks and best regards,  
<br> <br> <br> <br> <br> <br>


Chong Teng Sheng  
Project Sponsor, S&TPPO  
<chong_teng_sheng@pmo.gov.sg>



