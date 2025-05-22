![[Pasted image 20250427162914.png]]
# 数据集-小型

/home/arily/LargeD/ice-score/data/conala/conala_grade.json

这里面就有，

```

        "intent": "send a signal `signal.SIGUSR1` to the current process",

        "snippet": [

            "os.kill(os.getpid(), signal.SIGUSR1)"

        ],

        "baseline": "os.system('<unk>.png',s = 300)",

        "tranx-annot": "sys.signal(`signal.SIGUSR1`)",

        "best-tranx": "os.system(`signal.SIGUSR1`)",

        "best-tranx-rerank": "os.system(`< unk > < unk > < unk >`)",

        "codex": "os.kill(os.getpid(), signal.SIGUSR1)",

        "grade-baseline": 0,

        "grade-tranx-annot": 4,

        "grade-snippet": {

            "grader4": 4,

            "grader8": 4,

            "grader3": 1,

            "grader16": 4

        },

        "grade-best-tranx": 1,

        "grade-best-tranx-rerank": 0,

        "grade-codex": 3,

```

比如这样一个ground truth，snippet不用管，别的就是对应的case和grade（ground truth）。

  

# 数据集·大型

1. /home/arily/LargeD/LLM/JudgeBench/data 里的livebench code

2. CJEval

有关CJEval的标签，看https://yuanbao.tencent.com/chat/naQivTmsDa/f2641cc3-7a82-4c10-b23f-1a4d85f0df2c

# Mutation

/home/arily/LargeD/Tool/Python_refactor/generate_refactoring.py

看这里面就好，已经做好了。

  
# 攻击策略
可以考虑用训练模型评估下一步mutation（就是RL啦）
# 评估参考

CodeJudge

ICE-Score

RewardBench

[JudgeLLM 鲁棒性与偏好](https://chatgpt.com/c/680dd1d2-0dd4-8012-8933-9e17485b0bfc)

# Reward
目前采用 LLM评估改进作为单一reward
后面可以可考虑使用目标 LLM做代码摘要 ，评估摘要的perplexity或者语义相似度（对标CodeBLEU）