# 04 · API 清单

本文汇总业务 API 通道（Maimai2Servlet）已观察到的全部接口，按用途分组。

## 一、接口总览与索引

协议实现为每个接口分配了**统计索引**（0~40，共 41 项），用于登出时上报性能统计（delayLog，见 05）。索引表如下：

| 索引 | 接口名 | 索引 | 接口名 |
|---|---|---|---|
| 0 | GetUserDataApi | 21 | GetUserIntimateApi |
| 1 | GetUserCharacterApi | 22 | GetUserShopStockApi |
| 2 | GetUserItemApi | 23 | GetUserKaleidxScopeApi |
| 3 | GetUserFavoriteApi | 24 | UploadUserPlaylogListApi |
| 4 | GetUserGhostApi | 25 | UpsertUserAllApi |
| 5 | GetUserMapApi | 26 | UpsertClientSettingApi |
| 6 | GetUserLoginBonusApi | 27 | UpsertClientTestmodeApi |
| 7 | GetUserRegionApi | 28 | UpsertClientBookkeepingApi |
| 8 | GetUserRecommendRateMusicApi | 29 | GetGameEventApi |
| 9 | GetUserRecommendSelectMusicApi | 30 | GetGameChargeApi |
| 10 | GetUserOptionApi | 31 | GetGameRankingApi |
| 11 | GetUserExtendApi | 32 | GetGameNgMusicIdApi |
| 12 | GetUserRatingApi | 33 | GetGameNgWordListApi |
| 13 | GetUserMusicApi | 34 | GetGameKaleidxScopeApi |
| 14 | GetUserPortraitApi | 35 | GetGameWeeklyDataApi |
| 15 | GetUserActivityApi | 36 | GetGameTournamentInfoApi |
| 16 | GetUserFavoriteItemApi | 37 | Ping |
| 17 | GetUserRivalDataApi | 38 | UserLoginApi |
| 18 | GetUserRivalMusicApi | 39 | GetUserPreviewApi |
| 19 | GetUserMissionDataApi | 40 | UserLogoutApi |
| 20 | GetUserFriendBonusApi | | |

> 索引表之外的接口同样存在（见文末），说明 41 项索引仅覆盖统计需要的主流接口，并非全量边界。

## 二、会话类接口

| 接口 | 请求关键字段 | 响应关键字段 | 说明 |
|---|---|---|---|
| Ping | 空 | - | 连通性探测，机台级 |
| GetUserPreviewApi | userId, token, clientId | isLogin, userName, playerRating, lastRomVersion, lastDataVersion, banState, errorId | 登录前预检：判断玩家是否已在其他机台登录、读取其客户端版本（版本探测依据） |
| UserLoginApi | userId, accessCode, regionId, placeId, clientId, dateTime, loginDateTime, isContinue, genericFlag, token | returnCode, loginId, lastLoginDate | 建立会话；`isContinue` 区分首次登录与续关（详见 05） |
| UserLogoutApi | userId, accessCode, regionId, placeId, clientId, loginDateTime, type, delayLog | returnCode | 结束会话；`type` 区分登出性质（详见 05） |

## 三、机台上报类接口（机台级请求）

| 接口 | 请求关键字段 | 说明 |
|---|---|---|
| UpsertClientSettingApi | clientSetting: placeId, clientId, regionId, regionName, placeName, romVersion, isAou, isDevelop | 上报机台设置（门店、区域、ROM 版本、是否 AOU 机台） |
| UpsertClientTestmodeApi | clientTestmode: placeId, clientId, trackSingle, trackMulti, trackEvent, totalMachine, satelliteId, cameraPosition | 上报机台测试模式状态 |
| UpsertClientBookkeepingApi | - | 机台记账上报 |

## 四、游戏内容类接口（机台级请求）

| 接口 | 请求关键字段 | 响应关键字段 | 说明 |
|---|---|---|---|
| GetGameEventApi | type, isAllEvent | gameEventList | 游戏活动列表 |
| GetGameChargeApi | isAll | gameChargeList | 票务商品定义 |
| GetGameRankingApi | type | gameRankingList | 排行榜 |
| GetGameNgMusicIdApi | 空 | musicIdList | 禁用乐曲 ID 列表 |
| GetGameNgWordListApi | 空 | ngWordExactMatchList, ngWordPartialMatchList | 屏蔽词表（精确/部分匹配） |
| GetGameKaleidxScopeApi | 空 | gameKaleidxScopeList | 万花筒之门游戏侧定义 |
| GetGameWeeklyDataApi | 空 | gameWeeklyData | 周常活动数据 |
| GetGameTournamentInfoApi | 空 | gameTournamentInfoList | 锦标赛信息 |

## 五、用户基础数据类接口（玩家级请求）

| 接口 | 请求关键字段 | 响应关键字段 | 说明 |
|---|---|---|---|
| GetUserDataApi | userId | userData | 玩家基础档案（等级、Rating、金币等） |
| GetUserExtendApi | userId | userExtend | 玩家扩展数据 |
| GetUserOptionApi | userId | userOption | 玩家偏好设置 |
| GetUserRatingApi | userId | userRatingList | 单曲 Rating 明细 |
| GetUserActivityApi | userId | userActivityList | 活动参与数据 |
| GetUserMissionDataApi | userId | userMissionDataList | 任务进度 |
| GetUserRegionApi | userId | userRegionList | 区域解锁状态 |
| GetUserCharacterApi | userId | userCharacterList | 角色数据 |

## 六、用户内容数据类接口（玩家级请求）

| 接口 | 请求关键字段 | 响应关键字段 | 说明 |
|---|---|---|---|
| GetUserMusicApi | userId, nextIndex, maxCount | userMusicList[].userMusicDetailList[], nextIndex | 乐曲成绩（**分页**，游标见 05） |
| GetUserItemApi | userId, nextIndex, maxCount | userItemList, nextIndex | 物品库存（按类目分段分页） |
| GetUserMapApi | userId, nextIndex, maxCount | userMapList | 地图数据 |
| GetUserLoginBonusApi | userId, nextIndex, maxCount | userLoginBonusList | 登录奖励 |
| GetUserFavoriteApi | userId, itemKind | userFavoriteList | 收藏（按类目） |
| GetUserFavoriteItemApi | userId, kind, isAllFavoriteItem | userFavoriteItemList | 收藏物品 |
| GetUserPortraitApi | userId | userPortraitList | 头像框/名牌 |
| GetUserGhostApi | userId | userGhost | 幽灵数据 |
| GetUserIntimateApi | userId | userIntimateList | 亲密度数据 |
| GetUserShopStockApi | userId | userShopItemStockList | 商店库存 |
| GetUserKaleidxScopeApi | userId | userKaleidxScopeList | 万花筒之门玩家进度 |
| GetUserFriendBonusApi | userId | - | 好友加成 |

## 七、对手与推荐类接口

| 接口 | 请求关键字段 | 响应关键字段 | 说明 |
|---|---|---|---|
| GetUserRivalDataApi | userId: 0, rivalId | userRivalData | 对手基础信息（**免登录态**，userId 传 0） |
| GetUserRivalMusicApi | userId: 0, rivalId, nextIndex, userRivalMusicLevelList, maxCount | userRivalMusicList[].userRivalMusicDetailList[], nextIndex | 对手乐曲成绩（分页） |
| GetUserRecommendRateMusicApi | userId | - | 推荐刷分曲 |
| GetUserRecommendSelectMusicApi | userId | - | 推荐选曲 |

## 八、写入类接口

| 接口 | 请求关键字段 | 说明 |
|---|---|---|
| UploadUserPlaylogListApi | 游玩记录列表 | 上传游玩记录（单曲/批量） |
| UpsertUserAllApi | userId, playlogId, isEventMode, isFreePlay, loginDateTime, userPlaylogList[], upsertUserAll{...} | **核心写入端点**：一次性上行玩家全量数据（20+ 段，结构见 06） |

## 九、索引表之外已观察到的接口

以下接口在真实流量与协议脚本中出现，但不在 41 项统计索引内：

- `GetUserChargeApi`：玩家票务数据（`userChargeList`）
- `GetUserCourseApi`：段位课程数据
- `UpsertUserChargelogApi`：票务写入（购票流水 + 库存）
- `GetGameSettingApi`：机台启动设置（真实机台抓包中对应模拟实现的 Ping）
- `GetUserGetPointApi`、`GetUserTradeItemApi`、`GetUserFavoritemusicApi`、`GetUserFriendSeasonRankingApi`、`GetUserShopItemStockListApi` 等

> 说明：真实机台 URL 可能带额外路径段与 `Cookie`（JSESSIONID 会话）等头字段，协议实现复现时未必完全一致——本文记录的是协议语义，而非逐字节报文。
