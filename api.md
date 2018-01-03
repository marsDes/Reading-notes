
# 接口规范
## 返回值结构
```
{
	code : xxx,  // 返回码，0:请求成功，非0:请求失败
	msg : 'xxx', // 返回消息
	data : {     // 正常返回时数据部分，格式与接口有关
		...
	}
}
```

## 通用返回码
| code | msg | 说明 |
|:-----:|:-----|:-----|
| 0 | OK | 请求成功 |
| 1 | 身份验证失败 | token非法，需要重新登录 |
| 2 | 参数错误 | 如缺少参数，参数超出指定范围 |
| 99999 | 系统异常 | 服务不可用 |

# Entity结构定义
## <span id="SingleChoice">SingleChoice</span>(单选题)
```
{
	type : 'SingleChoice',   		// 问题类型，单选题
	qid : x,	   					// 问题id
	source : 'x',  					// 问题出处，真题（湖北省-201704真题-第5小题）、教材题、模拟题。
	mainKonwledgePoint : 'x',   	// 主知识点
	viceKnowledgePoint1 : 'x',  	// 副知识点1
	viceKnowledgePoint2 : 'x',  	// 副知识点2
	score : x,  					// 分数
	difficulty : 1|2|3|4|5,			// 难度。1-易；2-中偏易；3-中；4-中偏难；5-难
	content : 'x', 					// 题干
	answer : 'x',		 			// 正确答案
	analysis : 'x',					// 解析
	correctRate: 'x',				// 全站正确率
	confusingAnswer: 'x',			// 易错答案
	options : [{    				// 选项列表
		optionTitle : 'x', 				// 选项名称
		content : 'x', 					// 选项内容
		isCorrect : 0|1,  				// 该选项是否正确。0-错误；1-正确。answer根据该值计算
	}, ...]
}
```
## <span id="essay">Essay</span>(文字题)
```
{
	type : 'Essay',  	 			// 问题类型，文字题
	qid : x,	   					// 问题id
	source : 'x',  					// 问题出处，真题（湖北省-201704真题-第5小题）、教材题、模拟题。
	mainKonwledgePoint : 'x',   	// 主知识点
	viceKnowledgePoint1 : 'x',  	// 副知识点1
	viceKnowledgePoint2 : 'x',  	// 副知识点2
	score : x,  					// 总分
	difficulty : 1|2|3|4|5,			// 难度。1-易；2-中偏易；3-中；4-中偏难；5-难
	content : 'x', 					// 题干
	answer: 'x',					// 答案
	analysis : 'x', 				// 解析
}
```
## <span id="judgeEssay">JudgeEssay</span>(判断论述题)
```
{
	type : 'JudgeEssay',   			// 问题类型，论述题
	qid : x,   						// 问题id
	source : 'x',  					// 问题出处，真题（湖北省-201704真题-第5小题）、教材题、模拟题。
	mainKonwledgePoint : 'x',   	// 主知识点
	viceKnowledgePoint1 : 'x',  	// 副知识点1
	viceKnowledgePoint2 : 'x',  	// 副知识点2
	score : x,  					// 总分
	difficulty : 1|2|3|4|5,			// 难度。1-易；2-中偏易；3-中；4-中偏难；5-难
	content : 'x', 					// 题干
	analysis : 'x', 				// 解析
	choice : {                    	// 判断部分
		score : x,                		// 分数
		difficulty : x,   	         	// 难度
		answer : 'x',       	      	// 答案
		options : [{    				// 选项列表
			optionTitle : 'x',  			// 选项名称
			content : 'x',  				// 选项内容
			isCorrect : 0|1,  				// 该选项是否正确。0-错误；1-正确
		}, ...]
	},
	essay : {						// 论述部分
		difficulty : x,					// 难度
		score : x,						// 分数
		answer : 'x',					// 答案
	}
}
```
## <span id="comprehensive">Comprehensive</span>(综合题)
```
{
	type : 'Comprehensive',  	 	// 问题类型，综合题
	qid : x,	   					// 问题id
	source : 'x',  					// 问题出处，真题（湖北省-201704真题-第5小题）、教材题、模拟题。
	score : x,  					// 总分
	difficulty : 1|2|3|4|5,			// 难度。1-易；2-中偏易；3-中；4-中偏难；5-难
	content : 'x', 					// 题干
	children: [{...},]				// 实际题目，类型可能为单选、文字、判断论述等。老师资格证综合题当前只有文字题。
}
```
# APP接口定义
## 用户相关

### 获取验证码
#### 请求地址
GET /user/smsCode
#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| mobile | string | 是 | 用户手机 |

---
#### 正确返回时data结构
```
{
	smsCode: x   // 验证码
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|
| xxx | xxx | 待定，与个人中心返回值相关|

---
### 校验验证码
#### 说明
- 验证码在一段时间内都有效，有效时间内可以多次使用。如调用该接口后，[注册](#register)接口可以继续使用。
#### 请求地址
POST /user/smsCode/check
#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| mobile | string | 是 | 用户手机 |
| smsCode | string | 是 | 验证码 |

---
#### 正确返回时data结构
```
{
	smsCode: x   // 验证码
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### <span id="register">注册</span>
#### 请求地址
POST  /user/register

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| mobile | string | 是 | 用户手机 |
| password | string | 是 | 密码 |
| smsCode | string | 是 | 验证码 |

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|
| xxx | xxx | 待定，与个人中心返回值相关|   

---
### 登录
#### 请求地址
POST /user/login

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| mobile | string | 是 | 用户手机 |
| password | string | 是 | 密码 |

---
#### 正确返回时data结构
```
{
	token : 'x',      // 后续接口验证身份用
	uid: x,			  // 用户id
	avatarUrl: 'x',	  // 用户头像URL
	nickname: 'x',    // 用户昵称，如果有
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|
   

---
### 登出
#### 请求地址
POST /user/logout

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 重置密码
#### 请求地址
POST /user/resetPassword

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| mobile | string | 是 | 用户手机 |
| newPassword | string | 是 | 新密码 |
| smsCode | string | 是 | 验证码 |

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 检查用户是否存在
#### 请求地址
POST /user/checkAccountIsExist

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| mobile | string | 是 | 用户手机 |

---
#### 正确返回时data结构
```
{
	isExist: 1	// 0：不存在；1：存在
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 在线心跳
#### 请求地址
POST /user/heartBeat

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
## 考试相关
### 获取支持的考试
#### 请求地址
GET /exam/exams

#### 请求参数
无

---
#### 正确返回时data结构
```
{
	exams: [{			// 考试列表
		examId: x,			// 考试id
		examName: 'x',		// 考试名
		subjects: [{		// 该考试下的科目
			subjectId: x,		// 科目id
			subjectName: 'x'	// 科目名
		}, ...]
	}, ...]
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 保存用户考试目标
#### 请求地址
POST /exam/target

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| token | string | 是 | header参数，登录时返回的token |
| examId | int | 是 | 考试id |
| subjectId | int | 是 | 科目id |

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 获取用户考试目标
#### 请求地址
GET /exam/target

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
{
	examId: x,		// 考试id
	subjectId: x,	// 科目id
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
## 做题相关

### 获取所有类型练习最近的练习进度
#### 说明
- 有进行中练习，返回进行中练习信息
- 无进行中练习，且有未做过练习，返回可做的下一练习信息
- 无进行中练习，且所有练习已完成，不返回该类型练习的数据
- 排序：最近练习排前；没练习时真题>章节>模拟

#### 请求地址
GET /subject/{subjectId}/progresses
#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| subjectId | int | 是 | 路径参数，科目id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
{
	progresses: [{	// 
		quizType: x,		// 练习类型：1-章节类型；2-智能练习；3-真题练习；4-模拟练习
		srcId: x,			// 题目源id，与练习类型相关，如章节id、试卷id；智能练习无该属性
		srcName: 'x',		// 题目源名称，如章节名、试卷名；智能练习无该属性
		questionCount: x,	// 题目数
		quizId: x,			// 练习id，有进行中练习才返回
		answerCount: x,		// 已答题数，有进行中练习才返回
	}, ...] 
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 获取指定科目的章节练习进度
#### 请求地址
GET /subject/{subjectId}/chapters/progress

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| subjectId | int | 是 | 路径参数，科目id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
{
	chapters: [{
		chapterId: x,		// 章节id
		chapterName: 'x',	// 章节名
		questionCount : x,  // 题目数
		quiz: {				// 该章节最近的练习信息，有进行过练习才返回
			quizId: x,			// 练习id
			status: x,			// 练习状态，1-练习中; 2-已完成(即已交卷)
			answerCount: x,		// 已答题数
		}
	}, ...]
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 练习某一章节
#### 请求地址
POST /quiz/create/chapter/{chaperId}

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| chapterId | int | 是 | 路径id，目标章节id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
{
	quizId: x,   // 新建的练习id
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### <span id="quizProgress">获取指定quiz的练习信息</span>
#### 说明
- 该接口会返回部分题目数据，需要通过[接口](#getQuizQuestions)获取其他题目数据。

#### 请求地址
GET /quiz/{quizId}/progress

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| quizId | int | 是 | 路径参数，目标quizId |
| token | string | 是 | header参数，登录时返回的token |
| questionSize | int | 是 | 返回的题目数量，需要分页加载题目元数据 |

---
#### 正确返回时data结构
```
{
	quizId: x,   		// 新建的练习id
	quizType: x,		// 练习类型：1-章节类型；2-智能练习；3-真题练习；4-模拟练习；5-错题练习；6-收藏练习
	name: 'x',			// quiz名称，如某章节名、试卷名
	status: x,			// quiz状态，1-练习中；2-已完成
	questions: [...]    // 题目元数据，章节、智能、错题、收藏只有单选题；真题、模拟有所有类型题目
	userAnswers: [{		// 用户答题卡，返回该练习所有题目，即使用户未作答
		questionId: x,		// 题目id
		questionType: 'x'	// 题目类型
		answer: 'x'			// 用户答案，单选题才有值
		isCorrect: 0|1,		// 用户答案是否正确，单选题才有值
	}, ...]
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### <span id="getQuizQuestions">获取指定quiz的题目信息</span>
#### 说明
- 部分章节有太多题目，如中学综合素质第五章有100+单选题，一次返回所有数据不科学。
- 该接口用于解决[获取练习进度](#quizProgress)接口的题目元数据分页问题。

#### 请求地址
GET /quiz/{quizId}/progress/questions

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| quizId | int | 是 | 路径参数，目标quizId |
| token | string | 是 | header参数，登录时返回的token |
| questionIds | int数组 | 是 | 希望返回的题目id数组，格式：id1,id2,id3 |

---
#### 正确返回时data结构
```
{
	questions: [...]    // 题目数据，章节、智能、错题、收藏只有单选题；真题、模拟有所有类型题目
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### <span id="answer">答题</span>

#### 说明
- 管理后台要求做题过程中就有用户答题记录，APP每次答题都可以提交答案吧。
- 服务端根据练习类型在统计时特别处理下，如统计答题数时过滤未交卷题目。

#### 请求地址
POST /quiz/{quizId}/answer


#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| quizId | int | 是 | 路径参数，目标quizId |
| token | string | 是 | header参数，登录时返回的token |
| answers | json | 是 | 待保存答案 |

##### data格式
```
[{					// 用户答案列表
	questionId: x,	// 题目id
	answer: "x",	// 用户答案
}, ...]
```

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### <span id="submit">交卷并查看报告</span>
#### 说明
- 交卷之前需要通过[答题](#answer)提交用户答案。
- 章节练习，有未完成题目交卷后还可以继续答题。
- 其他类型练习交卷后，该练习不能再作答。

#### 请求地址
POST /quiz/{quizId}/submit

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| quizId | int | 是 | 路径参数，目标quizId |
| token | string | 是 | header参数，登录时返回的token |

---
#### <span id="submitData">正确返回时data结构</span>
##### 章节练习
```
{	
	chapterId: x,		// 章节id
	chapterName: 'x',	// 章节名称
	questionCount: x,	// 总题数
	correctCount: x,	// 答对数
	wrongCount: x,		// 答错数
	noAnswerCount: x,	// 未答数
	correctRate: 'x',	// 正确率
}
```
##### 智能练习
```
{	
	questionCount: x,	// 总题数
	correctCount: x,	// 答对数
	wrongCount: x,		// 答错数
	noAnswerCount: x,	// 未答数
	correctRate: 'x',	// 正确率
}
```
##### 真题、模拟题
```
{	
	paperId: x,				// 试卷id
	paperName: 'x',			// 试卷名
	totalScore: x,			// 总分
	difficulty: x,			// 试卷难度
	choiceScore: {			// 单选题得分
		questionCount: x,		// 题目数
		totalScore: x,			// 总分
		userScore: x,			// 用户得分
	},
	essayScore: {			// 文字题得分
		questionCount: x,		// 题目数
		totalScore: x,			// 总分，无用户得分
	},
	judgeEssayScore: {		// 判断论述题得分
		questionCount: x,		// 题目数
		totalScore: x,			// 总分，无用户得分
	}
}
```
##### 错题、收藏
```
无
```

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 查看练习报告
#### 说明
- 章节、真题、模拟题练习完成后且未重新开始，用户可查看练习报告。
- 接口[交卷并查看报告](#submit)会计算答题结果，该接口则直接返回已计算后结果。

#### 请求地址
GET /quiz/{quizId}/report

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| quizId | int | 是 | 路径参数，目标quizId |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
参考交卷并查看报告的[返回结果](#submitData)

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 获取用户真题试卷答题进度
#### 请求地址
GET /subject/{subjectId}/realPapers/progress

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| subjectId | int | 是 | 路径参数，目标科目id |
| token | string | 是 | header参数，登录时返回的token |
| offset | int | 是 | 请求偏移，从0开始 |
| size | int | 是 | 数量 | 

---
#### <span id="listRealPapersProgressData">正确返回时data结构</span>
```
{
	papers: [{
		paperId: x,			// 试卷id
		paperName: 'x',		// 试卷名称
		difficulty: x,		// 试卷难度
		durationInMins: x,	// 考试时长
		questionCount: x,	// 题目数
		quiz: {				// 该试卷最近的练习信息，如果有
			quizId: x,			// 练习id
			status: x,			// 练习状态，1-练习中；2-已完成
			answerCount: x,		// 已答题数
			userScore: x,		// 用户得分，练习已完成才有值
		}
	}, ...]
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 练习某一真题试卷
#### 请求地址
POST /quiz/create/realPaper/{paperId}

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| paperId | int | 是 | 路径参数，目标试卷id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
{
	quizId: x,		// 新建的练习id
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 获取用户模拟题试卷答题进度
#### 请求地址
GET /subject/{subjectId}/mockPapers/progress

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| subjectId | int | 是 | 路径参数，目标科目id |
| token | string | 是 | header参数，登录时返回的token |
| offset | int | 是 | 请求偏移，从0开始 |
| size | int | 是 | 数量 |

---
#### 正确返回时data结构
[与真题一样](#listRealPapersProgressData)

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 练习某一模拟试卷
#### 请求地址
POST /quiz/create/mockPaper/{paperId}

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| paperId | int | 是 | 路径参数，目标试卷id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
{
	quizId: x,		// 新建的练习id
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 获取错题列表
#### 请求地址
GET /subject/{subjectId}/wrongSets

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| subjectId | int | 是 | 路径参数，目标科目id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
{
	chapters: [{		// 有错题的章节
		chapterId: x,		// 章节id
		chapterName: 'x',	// 章节名
		questionCount: x,	// 错题数
	}, ...]
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 错题练习
#### 请求地址
POST /quiz/create/wrongSet/{chapterId}

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| chapterId | int | 是 | 路径参数，目标章节id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
{
	quizId: x,		// 新建的练习id
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 获取收藏列表
#### 请求地址
GET /subject/{subjectId}/favorites

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| subjectId | int | 是 | 路径参数，目标科目id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
{
	chapters: [{		// 有收藏的章节
		chapterId: x,		// 章节id
		chapterName: 'x',	// 章节名
		questionCount: x,	// 题目数
	}, ...]
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 收藏练习
#### 请求地址
POST /quiz/create/favorites/{chapterId}

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| chapterId | int | 是 | 路径参数，目标章节id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
{
	quizId: x,		// 新建的练习id
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 智能练习
#### 请求地址
POST /quiz/create/smart

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| token | string | 是 | header参数，登录时返回的token |
| subjectId | int | 是 | 科目id |

---
#### 正确返回时data结构
```
{
	quizId: x,		// 新建的练习id
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 收藏题目
#### 请求地址
POST /favorites/{questionId}/collect

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| questionId | int | 是 | 路径参数，题目id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 取消收藏
#### 请求地址
POST /favorites/{questionId}/delete

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| questionId | int | 是 | 路径参数，题目id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 从错题本移除错题
#### 请求地址
POST /wrongSet/{questionId}/delete

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| questionId | int | 是 | 路径参数，题目id |
| token | string | 是 | header参数，登录时返回的token |

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### <span id="totalReport">查看总体报告</span>
#### 请求地址
GET /report/full

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| token | string | 是 | header参数，登录时返回的token |
| dailySize | int | 否 | 分日练习报告返回数量，默认10 |

---
#### 正确返回时data结构
```
{
	practiceDays: x,	// 累计练习天数
	durationInMins: x,	// APP累计使用分钟数
	answerCount: x,		// 答题量
	exceedRate: 'x',	// 打败多少考生
	correctRate: 'x'	// 正确率
	dailyReport: [{		// 每日练习报告
		date: 'x',			// 日期，格式yyyyMMdd
		durationInMins: x,	// 当日APP使用分钟数
		answerCount: x,		// 答题量
		correctRate: 'x',	// 正确率
	}, ...]
}
```

### 查看分日练习数据
#### 说明
- [总体报告](#totalReport)加载第一页后，该接口加载后续页面。

#### 请求地址
GET /report/daily

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| token | string | 是 | header参数，登录时返回的token |
| offset | int | 是 | 请求偏移，从0开始 |
| size | int | 是 | 数量 |

---
#### 正确返回时data结构
```
{
	dailyReport: [{		// 每日练习报告
		date: 'x',			// 日期，格式yyyyMMdd
		durationInMins: x,	// 当日APP使用分钟数
		answerCount: x,		// 答题量
		correctRate: 'x',	// 正确率
	}, ...]
}
```

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


## 其他
### 获取咨询列表
#### 请求地址
GET /information/{type}/informations

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| type | int | 是 | 路径参数，资讯类型，1-考试必看；2-最近更新 |
| offset | int | 否 | 请求偏移，从0开始|
| size | int | 否 | 数据量 |

---
#### 正确返回时data结构
```
{
	infos: [{			// 资讯列表
		infoId: x,			// 咨询id
		title: 'x',			// 标题
		publishTime: 'x',	// 资讯发布时间，格式yyyyMMdd
	}, ...]
}
```

---
### 获取指定咨询详情
#### 请求地址
GET /information/{infoId}

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| infoId | int | 是 | 路径参数，资讯id |


---
#### 正确返回时data结构
```
{
	title: 'x',
	content: 'x',		// 咨询内容
	source: 'x',		// 咨询来源
	publishTime: 'x',	// 资讯发布时间，格式yyyyMMdd
	
}
```

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 获取后端支持版本
#### 请求地址
GET /appMinVersion

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|


---
#### 正确返回时data结构
```
{
待定
}
```

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---

## 管理后台接口定义

### 查询用户信息
#### 请求地址
GET /users

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| mobile | string | 否 | 用户手机号 |
| fromTime | long | 否 | 首登时间范围开始点 |
| toTime | long | 否 | 首登时间范围结束点 |
| examId | int | 否 | 考试目标id |
| pageNo | int | 否 | 分页页码，从1开始 |
| pageSize | int | 否 | 分页大小 |


---
#### 正确返回时data结构
```
{
	total: x,		// 记录总数
	pageNo: x,
	pageSize: x,
	records: [{
		uid: x,					// 用户id 
		mobile: 'x',			// 手机号码
		firstLoginTime: 'x',	// 首次登录时间
		isNewRegister: 0|1,		// 1-新注册
		examName: 'x',			// 考试目标
		practiceDays: x,		// 练习天数
		answerCount: x,			// 答题量
		correctCount: x,		// 答对题量
	}, ...] 
}
```

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 导出用户信息
#### 说明
- 服务端限制可导出数据量，用户修改查询条件减少数据量。

#### 请求地址
GET /users/export

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| mobile | string | 否 | 用户手机号 |
| fromTime | long | 否 | 首登时间范围开始点 |
| toTime | long | 否 | 首登时间范围结束点 |
| examId | int | 否 | 考试目标id |

---
#### 正确时返回
excel文件

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 查询练习记录
#### 请求地址
GET /quizs

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| subjectId | int | 否 | 科目id |
| quizType | int | 否 | 练习类型：1-章节类型；2-智能练习；3-真题练习；4-模拟练习；5-错题练习；6-收藏练习 |
| fromTime | long | 否 | 练习时间范围开始点 |
| toTime | long | 否 | 练习时间范围结束点 |
| mobile | string | 否 | 用户手机号 |
| pageNo | int | 否 | 分页页码，从1开始 |
| pageSize | int | 否 | 分页大小 |

---
#### 正确返回时data结构
```
{
	total: x,		// 记录总数
	pageNo: x,
	pageSize: x,
	records: [{
		uid: x,					// 用户id 
		mobile: 'x',			// 手机号码
		subjectName: 'x',		// 科目名
		quizType: x,			// 练习类型：1-章节类型；2-智能练习；3-真题练习；4-模拟练习；5-错题练习；6-收藏练习
		quizName: 'x',			// 试卷、章节名
		quizSrcId: x,			// 试卷、章节id
		createTime: 'x',		// 开始时间
		questionCount: x,		// 总题量
		answerCount: x,			// 答题量
		correctCount: x,		// 答对题量
		quizStatus: x,			// 练习状态，1-练习中；2-已完成
	}, ...]
}
```

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 导出练习记录
#### 说明
- 服务端限制可导出数据量，用户修改查询条件减少数据量。

#### 请求地址
GET /quizs/export

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| subjectId | int | 否 | 科目id |
| quizType | int | 否 | 练习类型：1-章节类型；2-智能练习；3-真题练习；4-模拟练习；5-错题练习；6-收藏练习 |
| fromTime | long | 否 | 练习时间范围开始点 |
| toTime | long | 否 | 练习时间范围结束点 |
| mobile | string | 否 | 用户手机号 |
| pageNo | int | 否 | 分页页码，从1开始 |
| pageSize | int | 否 | 分页大小 |

---
#### 正确时返回
excel文件

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 查询答题记录
#### 请求地址
GET /answers

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| subjectId | int | 否 | 科目id |
| fromTime | long | 否 | 练习时间范围开始点 |
| toTime | long | 否 | 练习时间范围结束点 |
| mobile | string | 否 | 用户手机号 |
| questionId | int | 否 | 题目id |
| pageNo | int | 否 | 分页页码，从1开始 |
| pageSize | int | 否 | 分页大小 |

---
#### 正确返回时data结构
```
{
	total: x,		// 记录总数
	pageNo: x,
	pageSize: x,
	records: [{
		uid: x,					// 用户id 
		mobile: 'x',			// 手机号码
		subjectName: 'x',		// 科目名
		questionId: x,			// 题目id
		difficulty: x,			// 难度。1-易；2-中偏易；3-中；4-中偏难；5-难
		answerTime: 'x',		// 开始时间
		userAnswers: 'x',		// 用户答案
		isCorrect: 0|1,			// 用户答案是否正确
	}, ...]
}
```

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|

---
### 导出练习记录
#### 说明
- 服务端限制可导出数据量，用户修改查询条件减少数据量。

#### 请求地址
GET /answers/export

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| subjectId | int | 否 | 科目id |
| fromTime | long | 否 | 练习时间范围开始点 |
| toTime | long | 否 | 练习时间范围结束点 |
| mobile | string | 否 | 用户手机号 |
| questionId | int | 否 | 题目id |
| pageNo | int | 否 | 分页页码，从1开始 |
| pageSize | int | 否 | 分页大小 |


---
#### 正确时返回
excel文件

---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 获取支持的考试
#### 请求地址
GET /exam/exams

#### 请求参数
无

---
#### 正确返回时data结构
```
{
	total: x,		// 记录总数
	pageNo: x,
	pageSize: x,
	records: [{		// 考试列表
		examId: x,			// 考试id
		examName: 'x',		// 考试名
		subjects: [{		// 该考试下的科目
			subjectId: x,		// 科目id
			subjectName: 'x'	// 科目名
		}, ...]
	}, ...]
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 获取所有科目
#### 请求地址
GET /exam/subjects

#### 请求参数
无

---
#### 正确返回时data结构
```
{
	subjects: [{
		subjectId: x,		// 科目id
		subjectName: 'x'	// 科目名
	}, ...]
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 新增科目
#### 请求地址
POST /exam/{examId}/subject/add

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| examId | int | 是 | 路径参数，考试id|
| subjectId | int | 是 | 科目id |

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 获取咨询列表
#### 请求地址
GET /informations

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| infoType | int | 否 | 资讯类型，1-考试必看；2-最近更新|
| pageNo | int | 否 | 分页页码，从1开始 |
| pageSize | int | 否 | 分页大小 |

---
#### 正确返回时data结构
```
{
	total: x,		// 记录总数
	pageNo: x,
	pageSize: x,
	records: [{		// 资讯列表
		infoId: x,			// 咨询id
		title: 'x',			// 考试名
		infoType: x,		// 资讯类型，1-考试必看；2-最近更新
		updateTime: 'x',	// 更新时间
		viewCount: x,		// 点击次数
		orderVal: x,		// 排序值
	}, ...]
}
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 删除资讯
#### 请求地址
POST /information/{infoId}/delete

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| infoId | int | 是 | 路径参数，资讯id|

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 修改资讯排序
#### 请求地址
POST /information/{infoId}/order

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| infoId | int | 是 | 路径参数，资讯id|
| orderVal | int | 是 | 排序值|

---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


---
### 保存资讯
#### 请求地址
POST /information/save

#### 请求参数
| 参数名| 类型 | 必填 | 说明 |
|:-----:|:-----|:-----|:-----|
| infoType | int | 是 | 资讯类型 |
| infoId | int | 否 | 资讯id，新建咨询不需要；修改咨询需要 |


---
#### 正确返回时data结构
```
无
```
---
#### 错误码
| code | msg | 说明 |
|:-----:|:-----|:-----|


