\---

title: 'Tigase开发者指南'

date: 2016-11-28 03:41:37

tags: 后端

categories: 通信

\---

## 测试

### 测试

在开发过程中，测试是一个整个过程中的重要部分。

所有的release版本都是经过全自动化测试的。所有服务器的功能被实现都是通过测试后才会发布的。Tigase测试套件用于所有我们的自动测试，允许定义不同的测试场景。

我们对数据库测试没有进行调整。所有的数据库都以标准方式安装，并以默认方式运行。在每次循环测试之前，数据库数据都被清理。

同时，我们没有对默认配置进行更改。所有测试都是在配置向导生成的默认配置下执行的。

服务器在所有受支持的环境中进行测试：

- **XMLDB**,通过简单的XML database进行测试，对于小型服务安装，这是最简单和有效的方式。我们推荐它为100个用户的服务。虽然，我们对此通过了10000人的测试。

- **MySQL** ，使用mysql进行测试，虽然mysql比XMLDB慢得多，但是适用于更多的用户使用。

- **PostgreSQL** ，使用PostgreSQL数据库进行测试。再次，它比XMLDB慢得多，但可以处理更多的用户帐户。这与mysql的代码基本相同，但执行测试以确保他可以在支持sql的数据库上成功运行和性能比较。

- **Distributed** ，为分布式安装的测试。其中c2s和s2s组件在分离的机器上运行，该机器使用外部组件协议（XEP-0114）与运行SessionManager的另一台机器连接。

  ### 功能测试

  检查所有的功能运行是否正确。当代码被推送到源代码仓库上时，记性自动化测试。

  | Version    | XMLDB                                    | MySQL                                    | PGSQL                                    | Distributed                              |
  | ---------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
  | 3.3.2-b889 | [00:00:12](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b889/func/xmldb/functional-tests.html) | [00:00:17](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b889/func/mysql/functional-tests.html) | [00:00:17](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b889/func/pgsql/functional-tests.html) | none                                     |
  | 3.3.2-b880 | [00:00:13](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b880/func/xmldb/functional-tests.html) | [00:00:15](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b880/func/mysql/functional-tests.html) | [00:00:15](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b880/func/pgsql/functional-tests.html) | None                                     |
  | 3.0.2-b700 | [00:00:22](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.0.2-b700/func/xmldb/functional-tests.html) | [00:00:24](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.0.2-b700/func/mysql/functional-tests.html) | [00:00:25](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.0.2-b700/func/pgsql/functional-tests.html) | [00:00:25](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.0.2-b700/func/sm-mysql/functional-tests.html) |
  | 2.9.5-b606 | [00:00:22](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.5-b606/func/xmldb/functional-tests.html) | [00:00:24](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.5-b606/func/mysql/functional-tests.html) | [00:00:24](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.5-b606/func/pgsql/functional-tests.html) | [00:00:24](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.5-b606/func/sm-mysql/functional-tests.html) |
  | 2.9.3-b548 | [00:00:22](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.3-b548/func/xmldb/functional-tests.html) | [00:00:23](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.3-b548/func/mysql/functional-tests.html) | [00:00:25](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.3-b548/func/pgsql/functional-tests.html) | [00:00:25](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.3-b548/func/sm-mysql/functional-tests.html) |
  | 2.9.1-b528 | [00:00:21](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.1-b528/func/xmldb/functional-tests.html) | [00:00:23](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.1-b528/func/mysql/functional-tests.html) | [00:00:24](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.1-b528/func/pgsql/functional-tests.html) | [00:00:25](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.1-b528/func/sm-mysql/functional-tests.html) |
  | 2.8.6-b434 | [00:00:21](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.6-b434/func/xmldb/functional-tests.html) | [00:00:24](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.6-b434/func/mysql/functional-tests.html) | [00:00:24](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.6-b434/func/pgsql/functional-tests.html) | [00:00:25](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.6-b434/func/sm-mysql/functional-tests.html) |
  | 2.8.5-b422 | [00:00:21](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.5-b422/func/xmldb/functional-tests.html) | [00:00:24](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.5-b422/func/mysql/functional-tests.html) | [00:00:24](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.5-b422/func/pgsql/functional-tests.html) | [00:00:26](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.5-b422/func/sm-mysql/functional-tests.html) |
  | 2.8.3-b409 | [00:00:27](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.3-b409/func/xmldb/functional-tests.html) | [00:00:29](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.3-b409/func/mysql/functional-tests.html) | [00:00:29](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.3-b409/func/pgsql/functional-tests.html) | [00:00:32](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.3-b409/func/sm-mysql/functional-tests.html) |
  | 2.7.2-b378 | [00:00:30](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.7.2-b378/func/xmldb/functional-tests.html) | [00:00:34](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.7.2-b378/func/mysql/functional-tests.html) | [00:00:33](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.7.2-b378/func/pgsql/functional-tests.html) | [00:00:35](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.7.2-b378/func/sm-mysql/functional-tests.html) |
  | 2.6.4-b300 | [00:00:30](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b300/func/xmldb/functional-tests.html) | [00:00:32](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b300/func/mysql/functional-tests.html) | [00:00:35](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b300/func/pgsql/functional-tests.html) | [00:00:39](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b300/func/sm-mysql/functional-tests.html) |
  | 2.6.4-b295 | [00:00:29](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b295/func/xmldb/functional-tests.html) | [00:00:32](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b295/func/mysql/functional-tests.html) | [00:00:45](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b295/func/pgsql/functional-tests.html) | [00:00:36](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b295/func/sm-mysql/functional-tests.html) |
  | 2.6.0-b287 | [00:00:31](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.0-b287/func/xmldb/functional-tests.html) | [00:00:34](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.0-b287/func/mysql/functional-tests.html) | [00:00:47](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.0-b287/func/pgsql/functional-tests.html) | [00:00:43](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.0-b287/func/sm-mysql/functional-tests.html) |
  | 2.5.0-b279 | [00:00:30](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.5.0-b279/func/xmldb/functional-tests.html) | [00:00:34](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.5.0-b279/func/mysql/functional-tests.html) | [00:00:45](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.5.0-b279/func/pgsql/functional-tests.html) | [00:00:43](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.5.0-b279/func/sm-mysql/functional-tests.html) |
  | 2.4.0-b263 | [00:00:29](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.4.0-b263/func/xmldb/functional-tests.html) | [00:00:33](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.4.0-b263/func/mysql/functional-tests.html) | [00:00:45](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.4.0-b263/func/pgsql/functional-tests.html) | [00:00:44](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.4.0-b263/func/sm-mysql/functional-tests.html) |
  | 2.3.4-b226 | None                                     | [00:00:48](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/functional-tests.html) | None                                     | None                                     |

  ### 性能测试

  检查所有的功能是否运行良好

  | Version    | XMLDB                                    | MySQL                                    | PGSQL                                    | Distributed                              |
  | ---------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
  | 3.3.2-b889 | [00:12:17](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b889/perf/xmldb/performance-tests.html) | [00:13:42](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b889/perf/mysql/performance-tests.html) | [00:17:10](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b889/perf/pgsql/performance-tests.html) | none                                     |
  | 3.3.2-b880 | [00:13:39](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b880/perf/xmldb/performance-tests.html) | [00:14:09](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b880/perf/mysql/performance-tests.html) | [00:17:39](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.3.2-b880/perf/pgsql/performance-tests.html) | None                                     |
  | 3.0.2-b700 | [00:10:26](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.0.2-b700/perf/xmldb/performance-tests.html) | [00:11:00](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.0.2-b700/perf/mysql/performance-tests.html) | [00:12:08](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.0.2-b700/perf/pgsql/performance-tests.html) | [00:24:05](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/3.0.2-b700/perf/sm-mysql/performance-tests.html) |
  | 2.9.5-b606 | [00:09:54](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.5-b606/perf/xmldb/performance-tests.html) | [00:11:18](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.5-b606/perf/mysql/performance-tests.html) | [00:37:08](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.5-b606/perf/pgsql/performance-tests.html) | [00:16:20](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.5-b606/perf/sm-mysql/performance-tests.html) |
  | 2.9.3-b548 | [00:10:00](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.3-b548/perf/xmldb/performance-tests.html) | [00:11:29](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.3-b548/perf/mysql/performance-tests.html) | [00:36:43](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.3-b548/perf/pgsql/performance-tests.html) | [00:16:47](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.3-b548/perf/sm-mysql/performance-tests.html) |
  | 2.9.1-b528 | [00:09:46](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.1-b528/perf/xmldb/performance-tests.html) | [00:11:15](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.1-b528/perf/mysql/performance-tests.html) | [00:36:12](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.1-b528/perf/pgsql/performance-tests.html) | [00:16:36](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.9.1-b528/perf/sm-mysql/performance-tests.html) |
  | 2.8.6-b434 | [00:10:02](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.6-b434/perf/xmldb/performance-tests.html) | [00:11:45](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.6-b434/perf/mysql/performance-tests.html) | [00:36:36](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.6-b434/perf/pgsql/performance-tests.html) | [00:17:36](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.6-b434/perf/sm-mysql/performance-tests.html) |
  | 2.8.5-b422 | [00:12:37](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.5-b422/perf/xmldb/performance-tests.html) | [00:14:40](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.5-b422/perf/mysql/performance-tests.html) | [00:38:59](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.5-b422/perf/pgsql/performance-tests.html) | [00:21:40](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.5-b422/perf/sm-mysql/performance-tests.html) |
  | 2.8.3-b409 | [00:12:32](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.3-b409/perf/xmldb/performance-tests.html) | [00:14:26](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.3-b409/perf/mysql/performance-tests.html) | [00:37:57](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.3-b409/perf/pgsql/performance-tests.html) | [00:21:26](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.8.3-b409/perf/sm-mysql/performance-tests.html) |
  | 2.7.2-b378 | [00:12:28](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.7.2-b378/perf/xmldb/performance-tests.html) | [00:14:57](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.7.2-b378/perf/mysql/performance-tests.html) | [00:37:09](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.7.2-b378/perf/pgsql/performance-tests.html) | [00:22:20](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.7.2-b378/perf/sm-mysql/performance-tests.html) |
  | 2.6.4-b300 | [00:12:46](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b300/perf/xmldb/performance-tests.html) | [00:14:59](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b300/perf/mysql/performance-tests.html) | [00:36:56](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b300/perf/pgsql/performance-tests.html) | [00:35:00](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b300/perf/sm-mysql/performance-tests.html) |
  | 2.6.4-b295 | [00:12:23](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b295/perf/xmldb/performance-tests.html) | [00:14:59](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b295/perf/mysql/performance-tests.html) | [00:42:24](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b295/perf/pgsql/performance-tests.html) | [00:30:18](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.4-b295/perf/sm-mysql/performance-tests.html) |
  | 2.6.0-b287 | [00:13:50](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.0-b287/perf/xmldb/performance-tests.html) | [00:16:53](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.0-b287/perf/mysql/performance-tests.html) | [00:48:17](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.0-b287/perf/pgsql/performance-tests.html) | [00:49:06](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.6.0-b287/perf/sm-mysql/performance-tests.html) |
  | 2.5.0-b279 | [00:13:29](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.5.0-b279/perf/xmldb/performance-tests.html) | [00:16:58](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.5.0-b279/perf/mysql/performance-tests.html) | [00:47:15](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.5.0-b279/perf/pgsql/performance-tests.html) | [00:41:52](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.5.0-b279/perf/sm-mysql/performance-tests.html) |
  | 2.4.0-b263 | [00:13:20](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.4.0-b263/perf/xmldb/performance-tests.html) | [00:16:21](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.4.0-b263/perf/mysql/performance-tests.html) | [00:43:56](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.4.0-b263/perf/pgsql/performance-tests.html) | [00:42:08](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/2.4.0-b263/perf/sm-mysql/performance-tests.html) |
  | 2.3.4-b226 | None                                     | [01:23:30](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/performance-tests.html) | None                                     | None                                     |

  ### 稳定性测试

  检查长时间运行是否运行良好。他必须在几小时内处理成千上万的请求。

  | Version    | XMLDB | MySQL                                    | PGSQL | Distributed |
  | ---------- | ----- | ---------------------------------------- | ----- | ----------- |
  | 2.3.4-b226 | None  | [16:06:31](http://docs.tigase.org/tigase-server/7.0.1/Development_Guide/html/tests/stability-tests.html) | None  | None        |

  ## Tigase DB模式解释

  对于最基本的模式，对于基本的外观和简明概要可以在[schema creation script](https://projects.tigase.org/projects/tigase-server/repository/revisions/master/entry/database/mysql-schema-4-schema.sql)中查看。然而，看到上面的脚本，我们很难理解他是如何工作的。是怎样访问数据的。其实，在所有的表中，只有三个基本的表保存了用户的数据。分别是`tig_users`,`tig_nodes`,`tig_pairs`。但我们还是不清楚Tigase是如何被组织的。

  在你可以理解Tigase XMPP Server数据库模式，如何工作，如何使用之前。理解它为什么这样运行，并且知道这样运行的目标是什么。让我们从API开始，因为这将给你最好的介绍。

  可以通过以下的方法来进行简单的访问：

  ```java
  void setData(BareJID user, String key, String value);
  String getData(BareJID user, String key);
  ```

  更多复杂的版本：

  ```java
  void setData(BareJID user, String subnode, String key, String value);
  String getData(BareJID user, String subnode, String key, String def);
  ```


同时，API中包含了很多方法，但随着版本的增加，API 或多或少有写变化。所有的API 方法的描述可以在 [UserRepository](https://projects.tigase.org/projects/tigase-server/repository/entry/trunk/src/main/java/tigase/db/UserRepository.java) 界面中Java 文档部分找到。因此，除了重要的方法，我们不打算在这里描述太多细节。

对于一些特别的用户，我们或多或少的处理过用户信息。处理过这种键值对信息。这种结构和想法使得我们使用这些API变得非常的简单与灵活。因此，添加一个新的插件或者组件就不需要对数据库进行操作。例如，添加一个新的表，数据库添加一个新版本等等。

**UserRepository**暴露了所有有关Tigase的代码，主要的组件与插件，这些模块简单的调用了set/get方法来存储和访问数据。

一个插件或者组件被独立开发变得相当容易。你可以选择相同的key去存储数据与信息。为了避免key冲突，我们使用数据库节点的方式。因此大多数模块提供了一个子节点来获取和保存数据。子节点通常使用XMLNS 或者一个唯一的字符串来确认。

节点像文件系统的目录，非常的小。节点也许包含了子节点。使得Tigase Database拥有嵌入的结构。并且，这种形式与文件结构非常的相似。它通过/来对每一个节点进行分析。你可以体验以下数据库结构:

```sql
user-name@domain  --> (key, value) pairs
                   |
               roster -->
                       |
                     item1 --> (key1, value1) pairs.
                       |
                     item2 --> (key1, value1) pairs.
```

因此，可以像下面这中方式来从roster中获取每一项的数据：

```java
getData("user-name@domain", "roster/item1", key1, def1);
```

这对开发者来说非常的方便。同时，开发者可以关注于模块的开发，而不必担心数据存储以及组织结构。特别是在快速开发的模式下，可以快速尝试不同的解决方案。在实际项目中，以不同的方式来访问用户的信息应该是高效的。同时，在一些特性的用例中，更多复杂的API的使用允许你直接访问数据库和存储数据。

现在一些嵌入的结构在SQL上面实现。但是初始化的时候，Tigase’s database使用XML结构实现。这种方式自然与简单。

在SQL数据库中，我们使用三个表来实现嵌套结构:

- **tig_users**-保存主要的用户数据，JID，可选的密码，活跃状态，创建时间和另外一些账户基本的数据。


其实，所有信息都可以存储在tig_pairs中，但出于性能考虑，这样做可以快速访问和快速查询。

- **tig_nodes**-一个实现层次结构的表。当Tigase向XML database中存储数据时，结构是相当复杂的。在SQL数据库中导致对数据的非常慢的访问，现在更多的扁平结构被大多数组件使用。请注意，每一个用户实例都有一个根节点，由root表示。
- **tig_pairs**-一个以键值对方式存储用户所有信息的表。

我们已经知道这些数据是如何被组织的。现在我们去学习如何直接使用sql语句去访问数据。

我们假设我们有一个用户*admin@test-d*,现在可以执行以下语句来检索用户的花名册：

```sql
select pval
  from tig_users, tig_pairs
  where user_id = 'admin@test-d' and
        tig_users.uid = tig_pairs.uid and
        pkey = 'roster';
```

然而，如果多个模块将数据存储到单个用户的花名册下。我们将取回多个结果，为了得到正确的结果，我们好需要知道一个特定的键的层次结构。正常情况下，用户的根节点位root节点。所以，查询如下面这样：

```sql
select pval
  from tig_users, tig_nodes, tig_pairs
  where user_id = 'admin@test-d' and
            tig_users.uid = tig_nodes.uid and
            node = 'root' and
            tig_users.uid = tig_pairs.uid and
           pkey = 'roster';
```

信息如何存储在**tig_pairs**中取决于特定的模块。xml信息如下：

```xml
<contact jid="all-xmpp-test@test-d" subs="none" preped="simple" name="all-xmpp-test"/>
```

## 基本信息

### Tigase Server元素

为了更加轻松的学会下面的代码以及专业术语。我们对Tigase Server的设计与实现做一个简要的说明。这份文档也包含最基本的接口实现，其可以作为示例代码引用。

逻辑上，所有的服务器代码可以分为三个模块**components**，**plug-ins**和**connectors**

- **Component**，它是Tigase server的主要元素。Component是可以有单独的地址，可以接受和发送stanzas，可配置和可响应各种各样的事件的一个代码块。Tigase服务器实现的Component有,*c2s connection manager*, *s2s connection manager*, *session manager*, *XEP-0114 - external component connection manager*, *MUC - multi user char rooms*.(c2s连接管理器，s2s连接管理器，会话管理器，XEP-0114 - 外部组件连接管理器，MUC )。
- **Plug-in**,通常是复杂处理XMPP 节的小段代码，它没有自己的地址，经过处理之后可以产生新的节。Plug-ins通常被会话管理组件，c2s连接器组件加载，Tigase Server插件有:电子名片，账户注册，节点处理以及认证功能等等很多。
- **Connector**，它是负责连接数据库的模块，负责存储和检索用户数据，Tigase 有两种连接器，认证连接器和用户数据连接器。他们相互独立，可以连接不同的数据源。Tigase中实现的连接器有：mysql连接器，xml连接器，*Drupal*数据库连接器，*LibreSource*数据库连接器。

每一个模块都有相应的接口API,你要做的就是实现这些接口。然后可以根据配置加载到服务器，还有可用的抽象类实现这些接口，使开发更容易。

下面是接口的列表，更多信息，你必须查看每个模块的想写概要。

#### 组件

下面是当你实现别的组件时，需要查看的接口：

- **tigase.server.ServerComponent**，component的最基本接口，所有组件必须实现它。
- **tigase.server.MessageReceiver**，这个接口继承自ServerComponent，当你需要接收数据包时，这个接口是必须实现的。实现这个接口的有会话管理器，c2s连接管理器等等。
- **tigase.conf.Configurable**，实现这个接口可以实现数据可配置。非必需的，实现此接口可以实现运行时动态配置。因为是动态更改，所以实现时，必须注意。
- **tigase.disco.XMPPService**，继承此类可以实现响应ServiceDiscovery请求。
- **tigase.stats.StatisticsContainer**，继承此类可以返回运行时的统计信息。任何对象都可以收集作业统计信息，并实现此接口保证统计信息将以混合的方式呈现给想要查看它们的用户。

相比实现上面的接口，我们推荐你继承已经存在的抽象类。继承自抽象类能让你更关注你想实现的功能。下面是大多数有用的抽象类：

- **tigase.server.AbstractMessageReceiver**-它实现了4个基本的接口

  `ServerComponent`, `MessageReceiver`, `Configurable` and `StatisticsContainer`，它使用自己的线程来管理数据队列，防止死锁。它提供的驱动来处理数据，无论什么时候数据包到达，他都会调用abstract void processPacket(Packet packet);来处理数据包。你必须在自定义组件中实现这个方法。如果你的组件想要发送数据包，你需要调用：

  ```java
  boolean addOutPacket(Packet packet)
  ```

方法。

- **tigase.server.ConnectionManager**-它是AbstractMessageReceiver抽象类的拓展，从他的名字就可以看出，它负责网络管理有关的工作。如果自定义组件需要直接从网络中发送和接受数据。你应该实现基类，这个抽象类关心网络，IO，以及重连，socket连接等所有工作。如果你拓展这个类，你肯定关心数据来源，从`MessageRouter`中，调用`abstract void processPacket(Packet packet);`来处理数据，调用abstract Queue processSocketData(XMPPIOService serv);来处理socket连接数据。

  #### Plug-in

  当前实现的所有Tigase插件都位于包中：tigase.xmpp.impl。您可以使用此代码作为示例代码库。有3种类型的插件，它们位于tigase.xmpp包中定义：

  - **XMPPProcessorIfc**- 最重要也是最基本的接口，这是最常见的插件类型，只在正常模式线处理节。它接收数据包，代表用户处理它们并返回结果节。
  - **XMPPPreprocessorIfc** -
  - **XMPPPostprocessorIfc** -

  ### Connector

  #### 数据，Stanzas，数据包 - 数据流和处理

  从网络中接收到的数据包，数据包是从tigase.io包中的代码以字节为单位从网络套接字中读取的。字节通过tigase.net包中的代码转换为字符。它们被放到XML解析器（tigase.xml），这将它们转换为XML DOM结构。

  服务器中的所有数据都是以xml dom格式交换的，因为xml的xmpp协议所使用的格式。我们使用[Tigase XML parser and DOM builder](https://svn.tigase.org/reps/tigase-xmltools/trunk/).来处理xml数据。

  每一个节点被存储在`tigase.xml.Element`中，每一个节点又包含很多个子节点和各种各样的属性。你可以通过api来访问这些数据。

  简单来说，最常见的操作元素包装在`tigase.server.Packet`类中,他的作用是数据交换。

  ## 为什么使用最近的JDK

  没有什么特别的原因，最近稳定版jdk更好，写代码的人有强迫症。下面列举出了一些原因：

  - 易于维护
  - 易于部署
  - 有效的开发

  ## 在Eclipse中开发Tigase Server

    安装环境就不介绍了。

  下载源代码：

  ```sh
  git clone https://repository.tigase.org/git/tigase-server.git
  ```

  或者使用Eclipse git方式自动下载

  ```shell
  git://repository.tigase.org/git/tigase-server.git
  ```

  ## 源码编译

  ### Tigase XMPP Server 5.2.0 and later - Compilation and Generating Distribution Packages

  ​

  ​

  ​