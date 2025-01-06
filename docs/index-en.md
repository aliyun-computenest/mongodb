<h1> Rapid deployment of the CoAI on Alibaba Cloud compute nest </h1>

<blockquote>
    <p><strong> Disclaimer </strong>: This service is provided by a third party. We try our best to ensure its security,
        accuracy and reliability, but we cannot guarantee that it is completely free from failure, interruption, error
        or attack. Therefore, the company hereby declares that it makes no representations, warranties or commitments
        regarding the content, accuracy, completeness, reliability, suitability and timeliness of the Service and is not
        liable for any direct or indirect loss or damage arising from your use of the Service; for third-party websites,
        applications, products and services that you access through the Service, do not assume any responsibility for
        its content, accuracy, completeness, reliability, applicability and timeliness, and you shall bear the risks and
        responsibilities of the consequences of use; for any loss or damage arising from your use of this service,
        including but not limited to direct loss, indirect loss, loss of profits, loss of goodwill, loss of data or
        other economic losses, even if we have been advised in advance of the possibility of such loss or damage; we
        reserve the right to amend this statement from time to time, so please check this statement regularly before
        using the Service. If you have any questions or concerns about this Statement or the Service, please contact us.
    </p>
</blockquote>

<h2> Overview </h2>

<p>Waline A simple and secure comment system.</p>
<p>
    GitHub address of this project: <a href='https://github.com/zmh-program/chatnio/blob/main/README_zh-CN.md'>https://github.com/zmh-program/chatnio/blob/main/README_zh-CN.md</a></p>

<h2> Prerequisites </h2>

<p><font style="color:rgb(51, 51, 51);"> To deploy a CoAI community edition service instance, you need to
    access and create some Alibaba Cloud resources. Therefore, your account must contain permissions for the following
    resources. </font><font style="color:rgb(51, 51, 51);"> </font><strong><font style="color:rgb(51, 51, 51);">
    Description </font></strong><font style="color:rgb(51, 51>: 51);">: this permission is required only when your account is a RAM account. </font></p>

<table>
<thead>
<tr>
<th><font style = " color:rgb(51, 51, 51);"> Permission policy name </font></th>
    <th><font style="color:rgb(51, 51, 51);"> Remarks </font></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td><font style="color:rgb(51, 51, 51);">AliyunECSFullAccess</font></td>
        <td><font style="color:rgb(51, 51, 51);"> Permissions to manage ECS </font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);">AliyunVPCFullAccess</font></td>
        <td><font style="color:rgb(51, 51, 51);"> Permissions to manage a VPC </font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);">AliyunROSFullAccess</font></td>
        <td><font style="color:rgb(51, 51, 51);"> Manage permissions for the Resource Orchestration Service
            (ROS) </font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);">AliyunComputeNestUserFullAccess</font></td>
        <td><font style="color:rgb(51, 51, 51);"> Manage user-side permissions for the compute nest service
            (ComputeNest) </font></td>
    </tr>
    </tbody>
    </table>

<h2> Billing instructions </h2>

<p><font style="color:rgb(51, 51, 51);"> The cost of CoAI deployment in computing nest
    mainly involves:</font></p>

<ul>
    <li><font style="color:rgb(51, 51, 51);"> selected vCPU and memory specifications </font></li>
    <li><font style="color:rgb(51, 51, 51);"> System disk type and capacity </font></li>
    <li><font style="color:rgb(51, 51, 51);"> Internet bandwidth </font></li>
</ul>

<p> This service requires ECS instance can access CoAI server from public network. </p>

<h2> Deployment Architecture </h2>

<p> This service is deployed on a single ECS instance. The architecture is as follows:</p>

<p><img src="./images/architecture_ecs_single.png" alt=""/></p>

<h2> Parameter description </h2>

<table>
    <thead>
    <tr>
        <th><font style="color:rgb(51, 51, 51);"> parameter group </font></th>
        <th><font style="color:rgb(51, 51, 51);"> parameter items </font></th>
        <th><font style="color:rgb(51, 51, 51);"> Description </font></th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td><font style="color:rgb(51, 51, 51);"> Service instance </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Service instance name </font></td>
        <td><font style="color:rgb(51, 51, 51);"> No more than 64 characters in length, must start with an English
            letter, and can contain numbers, English letters, dashes (-), and underscores (_)</font></td>
    </tr>
    <tr>
        <td></td>
        <td><font style="color:rgb(51, 51, 51);"> Region </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Region where the service instance is deployed </font></td>
    </tr>
    <tr>
        <td></td>
        <td><font style="color:rgb(51, 51, 51);"> Payment type </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Resource billing type: Pay-As-You-Go and Subscription </font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);"> ECS instance configuration </font></td>
        <td><font style="color:rgb(51, 51, 51);"> instance type </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Instance specifications available in the Availability Zone </font>
        </td>
    </tr>
    <tr>
        <td></td>
        <td><font style="color:rgb(51, 51, 51);"> Instance password </font></td>
        <td><font style="color:rgb(51, 51, 51);"> is 8-30 in length and must contain three items (uppercase letters,
            lowercase letters, numbers, ()'~! @#$%^& *-+ ={}[]:;,.? Special symbols in)</font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);"> Availability Zone configuration </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Availability Zone </font></td>
        <td><font style="color:rgb(51, 51, 51);"> Zone where the ECS instance is located </font></td>
    </tr>
    <tr>
        <td><font style="color:rgb(51, 51, 51);"> Select an existing / create a new Virtual Private Cloud (VPC) </font></td>
        <td><font style="color:rgb(51, 51, 51);"> ZChoose whether to create a new VPC (Virtual Private Cloud) </font></td>
    </tr>
    <tr>
        <td>Basic resource configuration</td>
        <td><font style="color:rgb(51, 51, 51);"> Private network IPv4 subnet </font></td>
        <td><font style="color:rgb(51, 51, 51);"> The VPC where the new resource is located. </font></td>
    </tr>
    <tr>
        <td></td>
        <td><font style="color:rgb(51, 51, 51);"> Switch subnet segment </font></td>
        <td><font style="color:rgb(51, 51, 51);"> The switch where the newly created resource is located. </font></td>
    </tr>
    <tr>
        <td>Basic resource configuration</td>
        <td><font style="color:rgb(51, 51, 51);">VPC ID</font></td>
        <td><font style="color:rgb(51, 51, 51);"> VPC where the resource is located </font></td>
    </tr>
    <tr>
        <td></td>
        <td><font style="color:rgb(51, 51, 51);"> Switch ID</font></td>
        <td><font style="color:rgb(51, 51, 51);"> Switch where the resource is located </font></td>
    </tr>
    </tbody>
</table>

<h2> Deployment process </h2>

<ol>
    <li> visit the compute nest CoAI<a
            href="https://computenest.console.aliyun.com/service/instance/create/ap-southeast-1?type=user&ServiceName=CoAI Community Edition">
        deployment link </a> and fill in the deployment parameters as prompted
    </li>
    <li> Select payment type
        <img src="images/img-1-en.png" alt=""/></li>
    <li> Enter instance parameters
        <img src="./images/img-2-en.png" alt=""/></li>
    <li> Fill in the zone and network parameters and click Next: Confirm Order <img src="./images/img-3-en.png" alt=""/><img src="./images/img-4-en.png" alt=""/></li>
    <li> Confirm all parameters and estimate price, click Create Now and wait for the service instance deployment to complete</li>
    <li> After the service instance is deployed, click the instance ID to go to the details page and click: <img
        src="./images/img-5-en.png" alt=""/>
    </li>
    <li> Click on the root avatar in the bottom left corner, then click on the backend management. Here you can perform various configurations, as well as manage customer dashboards, account management, notifications, model marketplace, and more. 
        <img src="./images/img-7.png" alt=""/> <img src="./images/img-8.png" alt=""/>
    </li>
    <li> Click on "Model Market," then click on "Add New Model." Enter "qwen-turbo" for the model ID, click "Submit," and then click "Save" in the upper right corner.
        <img src="./images/img-9.png" alt=""/>
    </li>
    <li> Click on Channel Settings, enter the name, select "通义千问 TongYi" as the type, click Add Model, select "qwen-turbo", and enter the secret key for accessing 通义千问 TongYi. You can refer to: 
        <a href="https://www.alibabacloud.com/help/en/model-studio/developer-reference/get-api-key">Obtain an API key</a>for instructions on how to obtain the secret key. The API Key in the "Account Settings" section of the document is the secret key. Click Confirm.
        <img src="./images/img-10.png" alt=""/><img src="./images/img-11.png" alt=""/>
    </li>
    <li> At this point, you have successfully configured the CoAI Community Edition and can start using it! You can refresh the page and click to exit the backend to test the integration status of the large model. You can also share your CoAI address with others for them to register an account and log in to use it. Additionally, you can charge and manage the usage of others. Feel free to explore more features!
        <img src="./images/img-12.png" alt=""/>
    </li>
</ol>