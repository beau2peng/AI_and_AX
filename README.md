# AI and AX: Building a Practical AI Workflow Platform

> Notes and lessons from building an AI workflow platform
> for real-world administrative and educational work.

[한국어 버전](history_of_aitool.md)



![AI_TOOL Architecture](images/aitool_detail.png)


I make my living in IT, so I have been doing my best to survive the recent wave of AI-driven change. Since ChatGPT appeared, new technologies and services have been emerging every month or two—and sometimes every week. Things are changing so quickly that I seem to spend most of my time studying, experimenting, and testing new tools without even noticing how fast time is passing. Part of me would like things to slow down for a while, but the world seems to be moving even faster.

I somehow ended up in a position where, while I am not exactly a CTO (Chief Technology Officer), I have to think about many of the same questions: where IT is heading, which direction we should take, and what we need to prepare for. During a period of rapid change like this, there are simply too many things to consider. In some ways, I feel busier now than when I was younger. AI is a convergence of many areas of information technology, and it is evolving so quickly that the traditional approach of waiting for the technology to mature, making a large investment, and then replacing everything at once no longer seems realistic. And as a relatively small organization, we do not exactly have the budget for that kind of strategy either.

In 2026, the transition toward Agent AI seemed particularly important. While thinking about how we could apply AX (AI Transformation) to actual work, I eventually ended up building a workflow server. This post is partly a chance to slow down and partly an attempt to leave a record of what I have learned along the way.





<p align="center">
<b>***Do We Really Need a Separate Work Platform for AI?***</b>
</p>


Our organization is a higher-education institution—in other words, a university—and most of our work is related to administration and education. We need to read large numbers of documents, find supporting evidence in regulations and laws, and work with many different document formats such as HWP, PDF, XLS, and PPT. The workflow server started from a simple question:

**How can AI actually support this kind of work?**

The system was designed so that AI can follow predefined procedures and use dedicated tools to read documents, search for information, create content, and convert files.


The AI systems we commonly use today—LLMs (Large Language Models)—are extremely capable at understanding context, organizing information, and generating drafts. However, simply generating text is not enough to achieve the practical goal of AX: making work easier for people. The final output of real office work is usually a file such as HWP, PDF, PPT, or Excel. That means the system must be able to handle those files directly and support practical document-processing functions such as tables, text styles, image insertion, and layout.

This led to a design in which **AI reasoning and file processing are separated**.

Dedicated tools were developed for different file formats and integrated into a utility server. The AI can then use those tools according to the user's request. In conservative public-sector organizations, there are also long-established conventions in wording, document structure, and presentation style. Search and reference functions are therefore needed to improve both the content and the format of AI-generated results. The central server manages tool versions and access ranges consistently, while guidance prompts help the AI understand the required procedures quickly.





<p align="center">
<b>***A Central Server Instead of Individual MCP Servers?***</b>
</p>




General-purpose LLMs such as Claude and ChatGPT are improving at a speed that is difficult to imagine. Their ability to understand and organize large numbers of documents in a short time and generate content based on user requests is something humans simply cannot match. However, organizations also have to consider internal approval processes, standardized report formats, organization-specific writing conventions, preservation of existing document structures, and access control for internal information.

To connect and manage the functions and information required for each type of work, I concluded that centralizing these capabilities was the more practical approach. Installing document-processing tools as MCP (Model Context Protocol) servers on every user's personal computer is also close to impossible. Applying a newly emerging technology directly to ordinary users who are not technical specialists would be a major risk. It would also mean that the performance and configuration of every user's PC would effectively become part of the AX cost.

For that reason, information retrieval and file-conversion capabilities were centralized on internal servers. Users only need to let their AI read a single "guidance prompt file" provided through the internal network, and the AI can then follow the defined workflow. This removes the need for complicated configuration on individual devices and allows AI to perform work within a controlled scope. 

Another advantage is that this architecture is not tied to a specific LLM. Claude, ChatGPT, Gemini, and open-source AI models can all follow the same workflow and use the same tools. This helps minimize maintenance risks caused by rapid changes in LLM technology. The platform can also be connected relatively easily to other systems such as internal regulations, related laws, registered templates, and administrative databases. It provides a foundation that can support everything from simple "one-click generation" tasks to advanced workflows in which experienced users analyze and compare accumulated institutional data.





![AI_TOOL Architecture](images/aitool_summary.png)



<p align="center">
<b>***Workflow: Helping AI Use the Right Tools Correctly***</b>
</p>




This approach reflects the rapid development of agent-based AI software. As agent-style tools such as "Hermes" have become more widespread, I began to think that from an AX perspective, model performance itself is only part of the problem. It is equally important to define **which tools an AI can use, under what rules, and in what sequence**. The concept of "Harness Engineering," which became increasingly visible in 2026, describes an approach in which tools, instructions, validation procedures, and execution rules are designed together as an environment in which AI can perform work.

The workflow server plays the central role in this environment. It is not simply a catalog containing a large collection of APIs. Instead, it analyzes the user's request, determines the most appropriate workflow, and provides the AI only with the instructions and specifications required for that task. The AI first checks the operating principles described in the guidance prompt. Rather than reading all available documentation, it receives only the rules, templates, input formats, and procedures selected by the workflow server.

For example, when a user asks:

> "Create meeting minutes about OOO."

the AI receives information about the registered HWP template through the workflow server. It then checks the JSON (JavaScript Object Notation) schema required to create the document and uses the Reference API to examine sample meeting minutes so that it can understand the expected structure, writing style, and level of detail. After generating the content, the AI converts it into JSON and sends it to the central server. The server validates the request, and the registered-template API generates the final document file and returns it to the user.


The major workflows currently defined are as follows.


## Reading Document Files

- Extract PDF, HWP, HWPX, Word, PowerPoint, Excel, and other document files into Markdown, text, or JSON.


## Image Interpretation

- Structure image content and visible text so that it can be used for search and document understanding.


## Creating Registered Document Templates

- Create documents using registered templates for official drafts, meeting minutes, contracts, analysis reports, planning documents, development plans, and other standardized forms.


## Reference Search

- For frequently used documents such as official drafts and meeting minutes, retrieve existing examples to understand structure, information density, table layouts, and style patterns that can improve the quality of newly generated documents.


## Editing Document Files

- Use format-specific JSON structures and dedicated editing functions for HWP, HWPX, XLSX, and PPTX files to create new files or modify existing documents.


## Creating Lecture Materials

- Use instructor-specific presentation templates and predefined slide layouts to generate lecture content as PowerPoint files.


## Generating Chart Images

- Receive tabular data in a common chart format and generate images that can be inserted into documents and presentations.


## HTML-to-PDF Conversion

- Convert HTML-based information materials and reports into PDFs for distribution.


## Internal Data Search

- Use registered query fields from internal systems to dynamically retrieve business data and condition-based statistics.


## Vector-Based Knowledge Search

- Search accumulated knowledge sources such as regulations, laws, administrative rules, plans, and other documents for evidence that is semantically related to a user's question.


The workflow therefore determines **what data should be used, which tools should be used, and in what order the work should be performed**. As the number of available tools increases, this coordination layer becomes increasingly important for maintaining consistent results and improving the efficiency of AI use. When workflows are centrally managed, users do not need to change the way they interact with the system whenever a new tool or business procedure is added. They simply continue instructing the AI to read the guidance prompt.





<p align="center">
<b>***Common Rules for Tool APIs***</b>
</p>



The workflow defines how a task should be performed, but the Utility Server is responsible for producing the actual result. Although the APIs for different document formats such as HWP, HWPX, XLSX, and PPTX are different, they follow the same operating principles. The AI first identifies the tool specified by the workflow and then checks that tool's help information and schema. The help information describes supported functions, usage procedures, and important precautions. The schema defines which fields must be provided and how they should be structured in JSON. Even when modifying an existing file, the document is first represented through the API using a predefined JSON schema so that the AI can understand exactly what should be changed and how much of the document is affected.


The common flow is as follows.

## 1. Select the Appropriate Workflow and Tool API

- Determine whether the user's request involves reading a document, creating a registered template, modifying an existing file, searching for information, or converting a file.


## 2. Check Service Status and Usage Instructions

- Use endpoints such as `health`, `help`, and `schema` to verify service status, supported functions, and input specifications.


## 3. Check Required Source Documents and References

- When modifying an existing document, first read its original structure.
- When creating a standardized document, check the registered template and available reference materials.


## 4. Build and Validate the Request JSON

- Structure the required content, tables, styles, charts, and search conditions according to the schema and submit the request for validation.


## 5. Send the Request to the API and Receive the Result

- Send the validated JSON and any required files to the API to perform generation, modification, search, or conversion.


## 6. Download and Review the Result

- Open the returned file or result data and verify that its structure, tables, images, and charts match the user's request.



![AI_UTILITY Architecture](images/aidocutil_detail.png)




<p align="center">
<b>***What Does the Utility Server Do?***</b>
</p>




The Utility Server is the execution layer that handles document formats that are difficult for AI to manipulate directly. It centralizes the tools required for practical work, including reading document files, generating new files, modifying existing documents, interpreting images, generating charts, and converting documents to PDF.


## Reading Various Document Formats

- The server reads many document formats, including HWP, HWPX, DOC, DOCX, PPT, PPTX, PPTM, XLS, XLSX, PDF, and TXT, and structures them as Markdown, plain text, or JSON. It can quickly extract only the main text when needed, but it can also extract tables, images, and document structure separately for more detailed processing. Images contained in a document can be separated from the main text. When necessary, Vision AI running on the server can analyze those images and combine the results with the document text so that the AI can understand the complete document.


## Creating HWP and HWPX Documents

- HWP is one of the most important document formats used by public-sector organizations in South Korea. Reports, meeting minutes, plans, and many other types of administrative documents are commonly produced in HWP format. Frontier AI services such as Claude and GPT have begun supporting the open HWPX format, but the majority of existing documents are still stored in the traditional HWP format. The HWP Tool API in the Utility Server supports direct modification and generation of these files without requiring the user to convert them first.


## Creating Registered Templates with Reference Data

- Frequently used documents can be generated from registered HWP template files. The number of available templates is still limited, but several standardized forms have already been registered and the collection is being expanded. The AI only needs to identify the appropriate template and fill in the required content. Elements such as approval lines, table layouts, and document styles do not need to be recreated. The Reference API provides examples of how similar documents are structured, what kind of language they use, and how their styles are configured. This helps maintain the quality of AI-generated documents at a practical level.


## Excel Status Tables and Charts

- The Excel Tool built into the Utility Server reads and modifies XLSX sheets, cells, styles, formulas, and merged-cell information. It is suitable for tasks such as updating only numeric values in an existing file or constructing a new summary table. The Chart API is separate from the document tools and is responsible for generating chart images. Based on chart information defined by the AI—such as categories, series, values, and colors—it can generate bar, line, pie, and other chart types as PNG images. These chart images can then be inserted into document formats such as HWP and PPT.


## PowerPoint Lecture Materials and Presentations

- The PPT Tool reads and modifies PPTX and PPTM files. For lecture-material production, which is one of the most common use cases, predefined slide templates created by the relevant department are embedded in the Utility Server and used as the preferred option. The AI can create new designs when necessary, but for standard lecture materials it usually selects the most appropriate predefined slide layout and fills it with text and images. Instructor-specific presentation templates are also available so that complete lecture materials can be generated using registered slide layouts.


## HTML-Based PDF Conversion

- HTML is one of the file formats that generative AI handles particularly well. The system can therefore create information materials, guides, and reports as HTML and then convert them into PDF files. Fonts, margins, images, and other layout elements can be adjusted before conversion, making the resulting PDFs suitable for printing and distribution.


## Internal Data Search and Vector-Database Knowledge Search

- The Search API handles data queries against internal systems. It provides both vector-database search for accumulated institutional knowledge and legacy-database search for statistics-oriented numerical work. For numerical data that should be available for institutional reporting, the Search API tells the LLM what information it is allowed to access and aggregates data according to the LLM's request. The data sent to an LLM should be structured differently from data used by traditional business systems. In a conventional system, Report A receives data specifically prepared for Report A, and Report B receives data specifically prepared for Report B.

That approach does not work well for an LLM.

Instead, personally identifiable information is removed from the underlying business database, and a new generalized data structure—let us call it "Z"—is exposed. The LLM first understands what information is searchable within that domain, then determines the aggregation method required by the user's request and sends that request to the Search API. This means that the data structure must be flexible enough to support many different reports within the same business domain.

Vector-database search works differently. Regulations, laws, and task-specific reference materials are divided into smaller chunks and stored in a search index. The system retrieves the portions that are semantically related to a question and uses them as supporting evidence for AI-generated answers or document drafts. Rather than relying only on retrieved snippets, the system can also return to the original document when necessary, improving both accuracy and traceability.





<p align="center">
<b>***What Kind of Work Can This Platform Support?***</b>
</p>



By combining RAG (Retrieval-Augmented Generation), which provides reliable reference information for LLM reasoning, with Document Tools that produce the final output, the platform can support many different types of work.

- Meeting materials and notes can be converted into standardized meeting-minutes documents.
- Multiple documents can be read, analyzed, and summarized to create reports, plans, and review materials.
- Repetitive work that involves modifying existing HWP documents can be automated.
- Lecture content written by instructors can be converted into PowerPoint lecture materials.
- Internal databases can be queried to analyze current conditions and generate aggregated results in the desired document format.
- Regulations and laws can be searched for supporting evidence when generating answers or documents.
- Generative AI can create HTML-based documents that are then converted to PDF for distribution.


Below are some examples that were implemented using this system.

1. An open-source AI model and the Search API were used to provide regulation and legal-information search functions inside an existing business system.

![AI_TOOLS_Example1](images/search_rules.png)

![AI_TOOLS_Example2](images/search_laws.png)



2. Internal data was searched and analyzed to generate statistical reports.

The LLM can analyze aggregated information and use the PPT Tool to create a complete PowerPoint file. The prompt used in this example was:

> "Analyze the current enrolled-student population for the first semester of 2025 by admission type and create a short report as a PowerPoint file."

![AI_TOOLS_Example3](images/test_ppt01.png)
![AI_TOOLS_Example3](images/test_ppt02.png)
![AI_TOOLS_Example3](images/test_ppt03.png)
![AI_TOOLS_Example3](images/test_ppt04.png)
![AI_TOOLS_Example3](images/test_ppt05.png)
![AI_TOOLS_Example3](images/test_ppt06.png)





<p align="center">
<b>***Where Do We Go From Here?***</b>
</p>




The most important role of this platform is not simply to say that "AI created something." The real goal is to produce outputs that people can actually review and use. To achieve this, the workflow determines the appropriate sequence of tasks, dedicated tools process actual document formats and templates, and reference/search systems strengthen both the content and the evidence behind the document. These three elements need to work together if an AI-generated draft is to become something close to a practical business deliverable.

Final review and judgment still belong to people. The system's role is to reduce repetitive organization and formatting work and to provide AI with data that matches the user's purpose so that large amounts of information—which were previously difficult to analyze—can be examined from different perspectives. Rather than aiming for grand automation, I believe there is value even in making one small document task easier for a user.

In the future, more and more work will be performed with the help of AI. That means data will increasingly need to be structured not only for humans, but also in forms that AI can understand. Individual business procedures will also need to be converted into workflows so that even users who are unfamiliar with a particular task can receive assistance from AI while still following the appropriate regulations and procedures. This will take time. But I believe that if enough of these small changes accumulate, they may eventually lead to meaningful AX transformation.


