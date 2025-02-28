# Introduce
* I am a .NET developer with four years of experience in web development. 
* While my primary focus is on backend technologies such as ASP.NET Core Web API, SQL Server, and Entity Framework, I also have a foundational understanding of frontend technologies like React and ASP.NET Core MVC.

* Throughout my career, I have worked in Agile development environments and have occasionally taken on full-stack development responsibilities to meet project requirements. I have contributed to building management systems for government projects, including a documentation management system.

* These diverse experiences have allowed me to adapt to different project complexities and collaborate effectively with cross-functional teams to deliver high-quality solutions

## what is a 'high-quality solution' in web development?
* https://www.acquia.com/blog/crucial-website-quality-assurance-factors
* https://www.ease.io/blog/14-metrics-every-quality-exec-should-monitor-how-to-calculate-them/
* https://www.kdci.co/outsourcing-blog/post/quality-assurance-in-web-development
* https://www.researchgate.net/publication/316883982_Quality_assurance_of_web_services_A_systematic_literature_review

# Describe your favorite/last project ?
* My favorite project would be my last one—a Document Management System for a government organization
* The goal was to improve transparency and efficiency in document management.

* What made this project particularly interesting for me was its **`technical diversity`** 
* -> The project involved working with both the .NET Framework and the unified .NET platform, integrating a CMS that supported both server-side rendering (SSR) and client-side rendering (CSR).
* -> it was built on DotNetNuke CMS, which supported two main modules
* -> the first module was an ASP.NET MVC 5 web application, responsible for structuring the UI based on different menu items. However, most of the UI for each view was rendered dynamically using a bundled React project.
* -> to ensure security and modularity, we introduced an intermediate layer built as a Web API on .NET Framework. This API acted as a bridge, receiving requests from the ASP.NET MVC 5 module and forwarding them to the ASP.NET Core Web API.
* -> The ASP.NET Core Web API then handled data retrieval by interacting with a WCF Client, which encapsulated stored procedure calls to SQL Server.
* -> The reason for this architecture was that DotNetNuke CMS only supported .NET Framework modules. 
* -> However, to build a modern, responsive, and efficient website with faster development cycles and a better user experience, we chose React for the front end. This approach allowed us to offload UI rendering from the CMS while maintaining compatibility with its limitations.

* I had the opportunity to explore various technologies and design patterns, such as: 
* _UnitOfWork pattern for better data management, FluentValidation to simplify validation logic, Automapper for seamless object mapping, React for the frontend, where we built a custom application, And extensive unit testing to ensure reliability_

* The project was developed by a team of eight, following the Agile process, where we used Jira for tracking tasks and Confluence for documentation. 
* In this project, I contributed as a full-stack developer, handling both backend and frontend development.

* Overall, this project was a great learning experience and helped me sharpen my technical skills while working in a collaborative environment.

# most difficult issue that you have faced ?
* -> One of the most difficult issues I faced was integrating a digital signature service into my web application. This was my first time working on such a feature, and the service provider’s documentation only covered integration with an ASP.NET MVC project, whereas my project was built with React and ASP.NET Core Web API.
* -> When I initially attempted the integration, I encountered HTTP communication failures between the React frontend and the ASP.NET Core Web API. To troubleshoot, I first set up a simple ASP.NET MVC project and successfully integrated the digital signature service as described in the documentation. Through this, I discovered that the service used WebSockets for communication between the desktop application and the website, while file uploads were handled via HTTP.
* -> With this understanding, I analyzed the minified JavaScript file that was included in the ASP.NET MVC view. I used ChatGPT to help unminify the script and rename variables to better understand the WebSocket events. This allowed me to figure out how to call the necessary functions correctly within my React project.
* -> Additionally, I used Fiddler to capture HTTP requests from the desktop application to my web system. This helped me identify missing request headers and ensure the communication worked as expected.
* -> Ultimately, by combining a structured debugging approach, reverse engineering, and utilizing debugging tools, I was able to successfully integrate the digital signature service into my project

# how to catch up on new/big project? (how to transfer? how to get knowledge? what if have no good documentation...)
- khi join 1 source code mới: summary about domain, business, technical; pull code to start project, debugger; work with small task; 

# the main technologies your project uses? What are the common features your project includes?
	
# how to handle production issue?
* -> ta nên get a stable copy of production before your deploy và chạy nó lên (_có 1 **CI/CD pipeline** sẽ rất là tiện_) đảm bảo how quickly we could get a working version deployed and available for users again
* -> Debugging problem
* -> document everything we do, encounter, and notice during the debugging process (_Ví dụ khi ta giao việc debug lại cho người khác họ sẽ không phải bắt đầu lại từ đầu)

## Structured Steps for Debugging 
* _though **`developer's assumptions`** are key as they have a history with the product, the **`logs`** never lie_
* _focus on what went wrong instead of trying to figure out who to blame_

### Step 1: Replicating the Problem (Tái hiện lỗi)
* -> thu thập những reports, error descriptions, or screenshots từ khách hàng
* -> xác định điều kiện để lỗi xảy ra (specific user action, input data, API request) và sử dụng Stage or local **enviroment** để tái hiện
* -> và cần **`confirm that the issue is reproducible consistently under the given conditions`**

### Step 2: Analyzing Logs and Metrics 
* -> **`Access Logs`**: Look at application logs, web server logs, database logs, and infrastructure logs for relevant time periods.

* -> **`Search log`** - ta sẽ cần collect **data that reveals anomalies** or **patterns contributing to the issue**
* _ta có thể **`Search for Indicators`** - Identify error codes, stack traces, or unexpected log entries_
* _however, it’s not easy to read through all of them by developers, ta có thể triển khai các **`tools to interpret log data`**, these will help to stay on top of **large-scale production applications**_
* _Ví dụ như: **ELK Stack (Elasticsearch, Logstash, Kibana)**, **Grafana**, **Prometheus**, **DataDog**, and **Splunk**_

* -> **`Analyze Metrics`**: Examine dashboards for system health parameters like latency, throughput, CPU/memory usage, and disk I/O.
* -> **`Compare Timeframes`**: Look at data before, during, and after the issue to identify differences.
* -> **`Correlate Events`**: Match the timeline of the issue with any recent deployments, configuration changes, or traffic spikes

### Step 3: Root Cause Analysis (RCA) 
* _trace the issue back to a specific root cause to **ensure a targeted fix**_
* -> **`Hypothesize Causes`**: Based on logs and metrics, generate possible explanations for the issue 
* -> **`Test Hypotheses`**: Experiment with isolated changes or scenarios to confirm or reject each hypothesis
* -> **`Map Dependencies`**: Analyze service interactions, dependency graphs, or shared resources to find weak points
* -> **`Trace Code Paths`**: Debug the code involved in the faulty functionality using breakpoints or instrumentation
* -> **`Validate Findings`**: Use automated tests or controlled experiments to **`confirm the root cause`**

* _Techniques that could help with RCA:_
* -> **The 5 Whys**: Ask "why" iteratively until the root cause is revealed.
* -> **Fishbone Diagram**: Categorize potential causes (e.g., people, process, technology).
* -> **Fault Tree Analysis**: Map out failure paths leading to the issue

### Step 4: Collaborating Across Teams
* _những complex production problems thường liên quan nhiều systems và teams; mang những expertise liên quan để identify and resolve the problem_
* _an additional set of eyes from a different angle will bring a different perspective to the table_
* -> **`Identify Stakeholders`**: Involve developers, QA testers, operations, product managers, and third-party vendors if needed.
* -> **`Centralize Communication`**: Use incident response tools or shared platforms for real-time updates and task assignments.
* -> **`Define Roles`**: Assign clear ownership for tasks such as debugging, testing, and deploying fixes.
* -> **`Maintain Documentation`**: Record findings and decisions to ensure everyone is aligned.
* -> **`Post-Mortem Discussions`**: Review the incident collaboratively to identify gaps in processes or tools.

### Step 5: Applying a Fix and Testing
* _Implement and deploy a robust solution safely, a well-tested fix ensures that the problem is resolved **without causing new issues**_
* -> **`Develop the Fix`**: Write the code, configuration, or operational change to address the root cause.
* -> **`Peer Review`**: Conduct thorough reviews to catch potential mistakes or overlooked scenarios.
* -> **`Test the Solution`**: Use unit tests, integration tests, and performance tests to validate the fix under real-world conditions.
* -> **`Deploy Gradually`**: Roll out the fix incrementally, using techniques like **`blue-green deployments`** or **`canary releases`** to minimize risk.
* -> **`Monitor Post-Deployment`**: Keep a close eye on metrics and logs after deployment to detect regressions or side effects

### Step 6: Preventative Measures and Continuous Improvement
* -> improve **Automated Testing Frameworks**, **Code Reviews**, **Monitoring and Alerts**, **Redundancy and Resilience** (handle failures gracefully with fallback mechanisms), **Training and Knowledge Sharing**, **Post-Mortem Culture**

## Real-Life Examples & Case Studies
### SaaS Application Outage
We had a team of 10 resources working on a project that was catering to tens and thousands of customers across the world. It was a usual feature fix cycle that happened on a Monday morning. 
Though we had proper processes, environments, and experiences in place, one of the developers mistakenly updated the wrong configuration files onto production. 
Everything worked on unit testing until a live customer reported that his data was not populating. Swiftly within the next 15 minutes, the issue was resolved, and found that the test environment’s database was pointed in the production configuration file. This could have been a flow-blown issue if the logs were interpreted in the wrong way. Glad our tools came in handy

### Netflix’s Misconfigured Rule
Netflix once faced an outage due to a misconfigured firewall rule. By reproducing the issue in a test environment, the team identified the faulty rule. They implemented automated validation tools in their CI/CD pipeline to prevent such errors in the future
