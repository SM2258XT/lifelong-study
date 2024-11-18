---
title: RBAC表的设计（2）：sql
updated: 2021-08-09T17:09:32
created: 2021-08-09T17:04:03
---

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for **sys_role**
-- ----------------------------
DROP TABLE IF EXISTS \`sys_role\`;
CREATE TABLE \`sys_role\` (
\`id\` int(11) NOT NULL AUTO_INCREMENT,
\`rolename\` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
\`roleinfo\` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
\`gmt_created\` datetime NULL DEFAULT NULL,
\`gmt_modified\` datetime NULL DEFAULT NULL,
PRIMARY KEY (\`id\`) USING BTREE,
UNIQUE INDEX \`index_rolename\`(\`rolename\`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 5 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Compact;

-- ----------------------------
-- Records of sys_role
-- ----------------------------
INSERT INTO \`sys_role\` VALUES (1, 'visitor', '访客', '2021-08-09 10:37:38', '2021-08-09 10:37:38');
INSERT INTO \`sys_role\` VALUES (2, 'customer', '顾客', '2021-08-09 10:37:39', '2021-08-09 10:37:39');
INSERT INTO \`sys_role\` VALUES (3, 'staff', '职员', '2021-08-09 10:37:39', '2021-08-09 10:37:39');
INSERT INTO \`sys_role\` VALUES (4, 'admin', '系统管理员', '2021-08-09 10:37:39', '2021-08-09 10:37:39');

SET FOREIGN_KEY_CHECKS = 1;
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for **sys_user**
-- ----------------------------
DROP TABLE IF EXISTS \`sys_user\`;
CREATE TABLE \`sys_user\` (
\`id\` int(11) NOT NULL AUTO_INCREMENT,
\`username\` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
\`password\` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
\`realname\` varchar(50) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
\`is_enabled\` tinyint(1) NULL DEFAULT 1 COMMENT '可用的',
\`is_credentials\` tinyint(1) NULL DEFAULT 1 COMMENT '有凭证的',
\`is_non_locked\` tinyint(1) NULL DEFAULT 1 COMMENT '没有锁定',
\`is_non_expired\` tinyint(1) NULL DEFAULT 1 COMMENT '没有过期',
\`gmt_created\` datetime NULL DEFAULT NULL,
\`gmt_modified\` datetime NULL DEFAULT NULL,
PRIMARY KEY (\`id\`) USING BTREE,
UNIQUE INDEX \`index_username\`(\`username\`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 7 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Compact;

-- ----------------------------
-- Records of sys_user
-- ----------------------------
INSERT INTO \`sys_user\` VALUES (4, 'jack', '666', '杰克', 1, 1, 1, 1, '2021-08-09 10:44:31', '2021-08-09 10:44:31');
INSERT INTO \`sys_user\` VALUES (5, 'tom', '666', '汤姆', 1, 1, 1, 1, '2021-08-09 10:44:32', '2021-08-09 10:44:32');
INSERT INTO \`sys_user\` VALUES (6, 'root', 'zhang', '张三', 1, 1, 1, 1, '2021-08-09 10:45:47', '2021-08-09 10:45:47');

SET FOREIGN_KEY_CHECKS = 1;
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for **sys_relation**
-- ----------------------------
DROP TABLE IF EXISTS \`sys_relation\`;
CREATE TABLE \`sys_relation\` (
\`id\` int(11) NOT NULL AUTO_INCREMENT,
\`userid\` int(11) NOT NULL,
\`roleid\` int(11) NOT NULL,
\`gmt_created\` datetime NOT NULL,
\`gmt_modified\` datetime NOT NULL,
PRIMARY KEY (\`id\`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 9 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Compact;

-- ----------------------------
-- Records of sys_relation
-- ----------------------------
INSERT INTO \`sys_relation\` VALUES (1, 6, 1, '2021-08-09 11:12:40', '2021-08-09 11:12:40');
INSERT INTO \`sys_relation\` VALUES (2, 6, 2, '2021-08-09 11:12:40', '2021-08-09 11:12:40');
INSERT INTO \`sys_relation\` VALUES (3, 6, 3, '2021-08-09 11:12:40', '2021-08-09 11:12:40');
INSERT INTO \`sys_relation\` VALUES (4, 6, 4, '2021-08-09 11:12:40', '2021-08-09 11:12:40');
INSERT INTO \`sys_relation\` VALUES (5, 4, 1, '2021-08-09 11:12:40', '2021-08-09 11:12:40');
INSERT INTO \`sys_relation\` VALUES (6, 4, 2, '2021-08-09 11:12:40', '2021-08-09 11:12:40');
INSERT INTO \`sys_relation\` VALUES (7, 4, 3, '2021-08-09 11:12:40', '2021-08-09 11:12:40');
INSERT INTO \`sys_relation\` VALUES (8, 5, 1, '2021-08-09 11:12:40', '2021-08-09 11:12:40');

SET FOREIGN_KEY_CHECKS = 1;
