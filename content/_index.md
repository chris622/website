---
date: "2022-10-24"
title: liu mengjie
type: landing
sections:

- block: about.avatar
  content:
    title: Biography
    username: admin
  id: about

- block: experience
  id: edu
  content:
    date_format: Jan 2006
    items:
    - company: UCAS 中国科学院大学 
      company_logo: org-gc
      company_url: "https://www.ucas.ac.cn/"
      date_end: "2024-06-01"
      date_start: "2019-09-01"
      description: |2-
          * **GPA 绩点**: 3.92/4.0
          * **Awards 奖项**：国家奖学金 | 中国科学院大学院长奖 | 三好学生 | 优秀学生干部 | 优秀共产党员 | 清华大学钱易环境奖
          * **Research 研究**：第一作者发表6篇SCI论文，1篇中文核心论文，参加2次学术会议，完成一项团体标准
          * **Position 学生工作**：党支部书记 | 党支部纪检委员 | 党小组长 | 文艺委员
      location: Beijing 北京 
      title: PhD student of Environmental Engineering | 环境工程博士在读
    - company: HUST 华中科技大学
      company_logo: org-x
      company_url: "https://www.hust.edu.cn/"
      date_end: "2019-07-01"
      date_start: "2015-09-01"
      description: |2-
          * **GPA 绩点**: 3.98/4.0
          * **Awards 奖项**：国家奖学金（2017 & 2018) | 三好学生（2016-2018）| 启明学院特优生 | 光华奖学金
          * **Research 研究**：作为项目负责人带领两次大学生创新项目，发表一篇中文论文 | Vanderbilt University 暑期研究项目，发表一篇论文
          * **Certification 证书**：英语六级（566）| 计算机四级 （网络技术）| 计算机三级 （网络技术）| 计算机二级 （C++）| 普通话证书 （一级乙等）
          * **Position 学生工作**：校学工处事务管理办公室助理 | 大学生创新项目组长| 学习委员 
      location: Wuhan 武汉
      title: Bachelor of Municipal Engineering | 给排水科学工程学学士
    title: Education 
    subtitle: 教育经历
  design:
    columns: "1"

- block: collection
  content:
    filters:
      featured_only: true
      folders:
      - publication
    title: Featured Publications
    subtitle: 代表论文
  design:
    columns: "2"
    view: card
  id: featured

- block: collection
  content:
    count: 3
    filters:
      exclude_featured: true
      folders:
      - publication
    text: |-
      {{% callout note %}}
      Quickly discover relevant content by [filtering publications](./publication/).
      {{% /callout %}}
    title: Recent Publications
    subtitle: 近期发表论文
  design:
    columns: "1"
    view: list
    
- block: collection
  content:
    count: 5
    filters:
      author: ""
      category: ""
      exclude_featured: false
      exclude_future: false
      exclude_past: false
      folders:
      - post
      publication_type: ""
      tag: ""
    offset: 0
    order: desc
    subtitle: ""
    text: ""
    title: Recent Posts
    subtitle: 最近更新
  design:
    columns: "2"
    view: compact
  id: posts

- block: portfolio
  content:
    buttons:
    - name: All
      tag: '*'
    - name: R
      tag: R
    - name: Model
      tag: Model
    - name: Other
      tag: Demo
    default_button_index: 0
    filters:
      folders:
      - project
    title: Projects
    subtitle: 数据分析项目
  design:
    columns: "1"
    flip_alt_rows: false
    view: showcase
  id: projects

- block: tag_cloud
  content:
    title: Popular Topics
    subtitle: 热点话题
  design:
    columns: "2"

- block: contact
  content:
    address:
      city: 北京
      country:  中国
      country_code: CN
      postcode: "100045"
      region: 
      street: 双清路18号
    contact_links:
    directions: 生态环境研究中心
    email: liumj1998@163.com
    form:
      formspree:
        id: null
      netlify:
        captcha: false
      provider: netlify
    phone: 888 888 88 88
    title: Contact
    subtitle: 联络方式
  design:
    columns: "2"
  id: contact

---
