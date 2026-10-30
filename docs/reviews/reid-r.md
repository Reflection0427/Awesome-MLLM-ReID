# ReID-R 来源与收录核查

核查日期：2026-10-07。收录分类：Core MLLM / LVLM ReID。

## 论文身份与资源

- 方法：ReID-R。
- 标题：Thinking Before Matching: A Reinforcement Reasoning Paradigm Towards General Person Re-Identification。
- 作者：Quan Zhang、Jingze Wu、Jialong Wang、Xiaohua Xie、Jianhuang Lai、Hongbo Chen。
- [官方论文页面](https://arxiv.org/abs/2604.19218v1)；首次提交时间为 2026-04-21。
- [固定版本官方 PDF](https://arxiv.org/pdf/2604.19218v1)。本条目依据该版本及其[官方 TeX 源码](https://arxiv.org/src/2604.19218v1)核查。
- Conference / Journal 填写 **arXiv 2026**：官方页面与 v1 正文未注明正式会议或期刊，不能根据 ACM 排版模板推断发表场所。
- 在上述官方页面、PDF 和 TeX 正文中未找到作者代码或项目链接，Resources 仅列 Paper。

## 方法与收录依据

ReID-R 直接将大视觉语言模型用于 person ReID 的身份判断，符合核心 ReID 分类。基础模型为 **Qwen2.5-VL-7B-Instruct**；训练采用 GRPO，分为两个阶段：

1. **Discriminative Reasoning Warm-up**：在含正负样本的对比任务中生成身份相关描述，通过 SigLIP 图文对齐构造多视角对比奖励，并结合匹配准确性与格式奖励。无需人工 CoT 或描述标注，但训练仍使用身份标签。
2. **Efficient Reinforcement Learning**：通过 Non-trivial Data Sampling 选取有信息量的样本，训练 query 与单个候选图像的逐对推理和二元身份判断。

出处：PDF 第 3-5 页，Section 3；基础模型与训练实现见第 6 页 Section 4.1。

## 实验协议与作者报告结果

PDF 第 5-6 页 Section 4.1 明确描述推理流程：标准 ViT 检索器先筛选每个 query 的 **Top-5** gallery 候选；ReID-R 对这五个候选逐对进行 VQA 判断，再将预测为同一身份的候选排在其他候选之前。因此下表属于该检索与重排流程的作者报告结果。

| Dataset | mAP (%) | Rank-1 (%) | 官方 PDF 来源 |
| :--- | ---: | ---: | :--- |
| Market1501 | 92.5 | 96.5 | 第 5 页 Table 2 |
| MSMT17 | 69.3 | 86.8 | 第 5 页 Table 2 |
| CUHK03 | 85.6 | 87.7 | 第 5 页 Table 2 |
| PRCC | 46.2 | 46.4 | 第 5 页 Table 3 |
| VC-Clothes | 78.8 | 90.2 | 第 5 页 Table 3 |

这些数值已与 PDF 原表及官方 TeX 对照，尚未独立复现实验。v1 未充分说明 CUHK03 的 detected/labeled 与划分、同判定候选间的排序规则及其余图库排序的处理，不能据此认定与所有外部 SOTA 表采用完全相同的协议。

训练数据来自 MSMT17、CUHK03、Market1501、PRCC；作者在第 5 页 Table 1 报告 NTS 后共 14,300 个样本、占原始数据规模的 20.9%。训练来源与五个评测数据集应分开理解。

## Framework 原图与权利记录

- 原图：**Figure 2**，ReID-R 的两阶段 pipeline。
- 官方 PDF：`https://arxiv.org/pdf/2604.19218v1`。
- PDF SHA256：`7418767c984913541d51ed5e1f2bbbf009cde7d6b78cc769862acf273b43ecca`。
- 页码：**4**，从 1 开始的 PDF 页序。
- 裁剪矩形：`[48.0, 80.0, 564.0, 241.0]`；单位为 PDF point（1/72 英寸），左上角为原点，格式为 `[x0, y0, x1, y1]`。
- 图片路径：[assets/frameworks/reid-r.png](../../assets/frameworks/reid-r.png)。
- 提取方式：使用仓库 `scripts/extract_figure.py --paper-id reid-r` 从官方 PDF 直接渲染裁剪，220 DPI，保存无损 PNG；仅裁去图外空白与正文图注，完整保留图内文本、箭头、判断标记及 `(a)/(b)` 子图编号。
- 署名：Quan Zhang、Jingze Wu、Jialong Wang、Xiaohua Xie、Jianhuang Lai、Hongbo Chen。
- 原图权利归论文作者及其他原始权利人；仓库 MIT License 不适用于该论文图。
- arXiv 页面所示许可为 [arXiv non-exclusive distribution license](https://arxiv.org/licenses/nonexclusive-distrib/1.0/)。这并非可自由复用的 CC 许可，因此通用 `license_url` 保留为 `null`，分发许可另行记录。

上述来源、哈希、页码、坐标、路径与权利信息也记录在 `data/papers.json` 的 `reid-r` 条目中。
