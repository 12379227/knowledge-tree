## 从gitlab从导出issue到CSV文件简要步骤

### 1. 生成PRIVATE-TOKEN 
http://git.xxx.com/profile/personal_access_tokens

### 2. 获得gourp id
curl --header "PRIVATE-TOKEN:xxx-token" http://git.xxx.com/api/v3/groups/ >group.json

### 3.获得project id
curl --header "PRIVATE-TOKEN:xxx-token" http://git.xxx.com/api/v3/groups/xxx-group-id/projects >projects.json

### 4. 导出所有的issue
curl --header "PRIVATE-TOKEN:xxx-token" http://git.xxx.com/api/v3/projects/xxx-project-id/issues?per_page=100&page=1  >issues.json

### 5. 将json转化为csv
- 写代码或者使用在线转化工具都可

### 6. 说明
- git.xxx.com 是git库的url
- xxx-token: 是git库的PRIVATE-TOKEN
- xxx-group-id: 是git库下某个group的group id
- xxx-project-id: 是git库某个project的project id
- 导出issue的时候可以带参数: 使用curl命令行貌似只能带一个参数, 直接写代码可以带多个参数
  - per_page: 默认情况下, 1页20个issues, 最多1页 100个issues
  - page: 获取第几页的issue, 从1开始
  - state=opened or state=closed: 状态是opened的issue还是closed的issue




