HIGH KODE — AI-FIRST BUSINESS

Master Architecture, Strategy & P0 Implementation Plan

Version 1.0 | 10 August 2026

Mục tiêu: chốt kiến trúc và lộ trình thực thi để High Kode trở thành Customer Zero của mô hình AI-First Business trước khi sản phẩm hóa hệ thống ra thị trường.

1. Executive Summary

High Kode trước hết phải trở thành chính một doanh nghiệp AI-First: ERPNext giữ sự thật và trạng thái doanh nghiệp; Custom Business Apps chứa nghiệp vụ riêng; high_kode_ai là lớp thực thi AI; AI Workforce thực hiện công việc theo phòng ban và Action; con người giữ quyền quản trị, phê duyệt và kiểm soát rủi ro.

DingDongBot và AIWeb là các SaaS đã tồn tại và có doanh thu. Chúng không phải sản phẩm sinh ra từ AI Workforce. Chúng tiếp tục độc lập về product architecture, nhưng có thể dần được đưa vào business operating architecture của High Kode để Sales, Marketing, Development, Support, Finance và các hoạt động khác được quản trị tập trung.

Chỉ sau khi High Kode vận hành thực tế bằng hệ thống này, các workflow chứng minh hiệu quả mới được chuẩn hóa và productize thành AI Business Platform/SaaS.

2. North Star

AI-First Business — xây một doanh nghiệp mà AI có thể thực hiện ngày càng nhiều hoạt động kinh doanh một cách có kiểm soát, audit được và đo lường được.

Nguyên tắc: Build High Kode → AI vận hành High Kode → đo lường → chuẩn hóa → productize → bán ra ngoài.

3. Hai kiến trúc phải tách biệt

3.1 Product Architecture

HIGH KODE
├── DingDongBot — SaaS độc lập
├── AIWeb — SaaS độc lập
└── AI Business Operating System — hệ thống vận hành tương lai

DingDongBot và AIWeb có backend/database/product lifecycle riêng; không phụ thuộc ERPNext, HUF hay high_kode_ai để tồn tại.

3.2 Business Operating Architecture

HIGH KODE
        ↓
ERPNext — System of Record
        ↓
Custom Business Apps — Business Logic
        ↓
high_kode_ai — AI Execution Layer
        ↓
AI Workforce
        ↓
Human Governance
        ↓
High Kode vận hành thực tế

4. Core Architecture

4.1 ERPNext — System of Record

ERPNext là nền tảng quản trị và nguồn sự thật: CRM, Sales, Projects, Accounting, HR, Support, KPI, workflow, permissions, reports và transaction/business records. AI không thay ERPNext.

4.2 Custom Business Apps — Business Logic

Chứa nghiệp vụ đặc thù của High Kode; các business rule ổn định, deterministic và audit được không nên phụ thuộc vào prompt.

4.3 high_kode_ai — AI Execution Layer

Lớp thực thi AI: agent runtime, planning, tool use, context/memory khi cần, autonomy policy, action execution, run logging và governance hooks. Đây không phải database chính.

4.4 high_kode_integrations — Integration Layer

Cổng kết nối giữa DingDongBot/AIWeb/các hệ thống ngoài và ERPNext: API, webhook, event ingestion, mapping, synchronization, retry/idempotency, authentication và integration audit.

DingDongBot ──┐
              ├── API / Webhook → high_kode_integrations → ERPNext
AIWeb ────────┘

AI không tự truy cập database SaaS để đồng bộ.

5. Data Boundary & Privacy

Nguyên tắc nền tảng: Business Operations Data ≠ End-Customer Content.

Có thể đồng bộ dữ liệu vận hành như Customer, Subscription, Plan, Revenue, Usage, Ticket và KPI để quản trị High Kode. Không mặc định đưa conversation, private messages hoặc dữ liệu khách hàng cuối của khách hàng B2B vào ERPNext/AI Workforce.

Nếu cần AI xử lý trực tiếp end-customer content, phải có data-access policy, tenant isolation, mục đích xử lý, retention, logging và contractual/privacy review riêng.

6. Data Access Policy — phải viết trước code

Xác định system/source of truth cho từng loại dữ liệu.

Liệt kê field được phép và field tuyệt đối không được sync.

Xác định chiều dữ liệu, trigger và tenant boundary.

Xác định quyền đọc/ghi, retention và deletion.

Xác định audit, authentication, retry và reconciliation.

Kiểm tra ToS/privacy hiện hành trước khi mở sync.

7. AI Action & Autonomy Registry — phải viết trước code

Autonomy level thuộc về Action, không thuộc về Agent. Một Sales Agent không mặc nhiên được làm mọi việc của Sales.

8. Human Governance

Autonomy tăng dần: Shadow Mode → Human Approval → Controlled Autonomous → Higher Autonomy.

Mỗi Action phải có policy, điều kiện, approval requirement và audit trail. Hành động ảnh hưởng tiền, quyền truy cập, dữ liệu khách hàng, subscription, refund hoặc production deployment phải có human gate cho tới khi chứng minh đủ an toàn.

9. AI Workforce

AI Workforce là lực lượng AI thực hiện công việc của High Kode, không phải một product line riêng.

Agent chỉ được thực hiện các Action đã có trong Registry.

9.1 Architecture & Licensing Decision — HUF

Decision date: 10 August 2026. From this decision point, high_kode_ai is treated as an independently implemented High Kode custom app and is not based on HUF implementation code.

HUF was evaluated as an existing AI/agent framework that had been installed experimentally on ERPNext. The project documentation/license signals were not sufficiently unambiguous for a production SaaS strategy: the README/LICENSE information was understood to contain a potential MIT versus AGPLv3 conflict that had not yet been authoritatively clarified with the upstream maintainer. Because High Kode's long-term objective is a commercial SaaS platform, this unresolved licensing ambiguity is considered an unacceptable foundation for the core AI execution layer.

The architectural decision is therefore: do not make HUF a dependency of high_kode_ai and do not copy HUF implementation code. High Kode will independently implement the required AI execution capabilities from its own specification, architecture and codebase.

Clean-room / separation principle: HUF may be studied for general concepts and functional requirements, but implementation work for high_kode_ai should be driven by an independently written specification. Developers implementing high_kode_ai should not use HUF source code as a parallel implementation reference or copy its code, data model, prompts, naming, or implementation structure. This is a risk-control practice, not a guarantee of legal clearance.

Upstream clarification remains an open item: High Kode should ask Tridz for an explicit written statement of the applicable HUF license, commercial/SaaS usage rights, and any dual-licensing or commercial-license option. This inquiry does not change the current architecture decision; it closes the legal/strategic question for future reference.

Related legacy/custom code must be assessed separately. In particular, hospitality_core is treated as a separate licensing and provenance matter (including its GPL-2.0 history and High Kode modifications). Its repository history, upstream license obligations and relationship to the new High Kode apps should be documented independently rather than assumed to be covered by the HUF decision.

10. Customer Zero

High Kode là Customer Zero. P0 dùng hệ thống trên hoạt động thật của High Kode: Sales, Marketing, Development, Projects, Support, Finance, HR/Operations và KPI. DingDongBot và AIWeb được đưa vào phạm vi quản trị ở mức business operations phù hợp.

11. DingDongBot & AIWeb

Hai sản phẩm tiếp tục độc lập về product architecture. Hệ thống vận hành của High Kode quản trị business xung quanh chúng.

DingDongBot / AIWeb
        ↓
Business Operations Data
        ↓
high_kode_integrations
        ↓
ERPNext
        ↓
AI Workforce
        ↓
Sales / Marketing / Development / Support / Finance

AI Workforce không tạo ra các sản phẩm; nó giúp High Kode vận hành business quanh các sản phẩm đã có.

12. Ví dụ vòng đời nghiệp vụ

Lead mới
→ ERPNext ghi nhận
→ Sales Agent qualify
→ Policy kiểm tra
→ Action được phép thực thi
→ Human approval nếu cần
→ AI thực hiện
→ ERPNext cập nhật
→ KPI / audit ghi nhận
→ Event tiếp theo kích hoạt workflow

Mục tiêu dài hạn: ERP không chỉ ghi nhận việc con người làm; ERP + AI có thể phát hiện việc cần làm, thực hiện trong phạm vi được phép và cập nhật trạng thái doanh nghiệp.

13. P0 Implementation Plan

Viết và khóa Data Access Policy.

Viết và khóa AI Action & Autonomy Registry.

Dựng server sạch chỉ với Frappe + ERPNext.

Thiết lập repository, CI/CD, secrets và backup.

Tạo high_kode_integrations với phạm vi tối thiểu.

Tạo high_kode_ai v0.1 với action execution tối thiểu.

Kết nối một nguồn dữ liệu thật và một workflow giá trị cao/rủi ro thấp.

Chạy Shadow Mode.

Đưa workflow sang Human Approval.

Đo Task Success Rate, Human Hours Saved, Cost per Task, Error Rate và ROI.

Chỉ sau khi ổn định mới mở autonomy.

Mở rộng từng workflow, không xây toàn bộ AI Workforce cùng lúc.

14. MVP kỹ thuật

Lát cắt đầu tiên phải chạy được: Trigger → Context → Action Selection → Tool Execution → ERP Update → Audit.

Memory nâng cao, flow builder trực quan, multi-provider abstraction phức tạp và capability khác chỉ thêm khi use case thật chứng minh nhu cầu.

15. Reliability & Cost Controls

Giới hạn số bước/tool calls cho mỗi run.

Giới hạn token, thời gian execution và budget theo workflow.

Fallback model/provider khi cần.

Graceful degradation khi LLM/integration lỗi.

Retry có giới hạn và idempotency.

Chống infinite loop.

Audit các execution quan trọng.

16. Shadow Mode & Evaluation

Agent có thể chạy song song với người thật và ghi đề xuất vào internal note/log. So sánh output với kết quả thực tế để đo accuracy và task success rate trước khi nâng autonomy.

17. KPI Customer Zero

18. Priority

P0 — Xây High Kode AI-First Business và vận hành thật.

P1 — Duy trì DingDongBot/AIWeb và tích hợp ở mức cần thiết cho business operations; không phá product architecture hiện hữu.

P2 — Productize hệ thống vận hành thành AI Business Platform/SaaS sau khi workflow được chứng minh.

19. Website & Go-to-Market sau này

Website High Kode là mặt tiền marketing/sales, không phải business operating system. Có thể tạo nhiều landing page cho từng sản phẩm/dịch vụ. Chúng là acquisition channels.

Khi platform đã thành hình, AI Business Operating System là sản phẩm ngôi sao; DingDongBot, AIWeb và các dịch vụ khác là product/solution/entry point tùy chiến lược.

License/provenance decisions are documented as architecture decisions, not left only in chat history.

high_kode_ai must maintain an independent codebase and provenance trail.

20. Nguyên tắc bất biến

ERPNext = System of Record.

Custom Apps = Business Logic.

high_kode_ai = AI Execution Layer.

high_kode_integrations = Integration Layer.

AI không tự đồng bộ database.

Business Operations Data ≠ End-Customer Content.

Autonomy thuộc về Action, không thuộc về Agent.

Human Governance xuyên suốt.

High Kode = Customer Zero.

Không productize trước khi chứng minh.

Policy phải đi trước implementation.

Không mở rộng scope chỉ vì vision lớn.

13.1 Legal / Commercialization Gate

Before external commercialization of high_kode_ai or the eventual AI Business Platform, High Kode should obtain a legal review of software licenses, clean-room/provenance records, third-party dependencies, data-processing obligations, customer contracts/ToS, privacy requirements and the proposed SaaS distribution model. Clean-room development is a risk-control measure and is not, by itself, a legal opinion or guarantee.

21. P0 Definition of Done

Data Access Policy được phê duyệt và version-controlled.

AI Action & Autonomy Registry được phê duyệt và version-controlled.

Server sạch Frappe + ERPNext hoạt động ổn định.

high_kode_integrations có mapping, retry và audit.

high_kode_ai v0.1 thực thi action có kiểm soát.

Ít nhất một workflow thật của High Kode chạy end-to-end.

Shadow Mode và Human Approval đã được thử nghiệm.

Action quan trọng có audit trail.

Không integration nào vượt Data Access Policy.

Có KPI baseline để quyết định bước tiếp theo.

The HUF decision should be retained as an Architecture Decision Record (ADR) with its evidence, dates, upstream correspondence and legal review status.

22. Kết luận

High Kode không xây một AI demo. High Kode xây chính mình thành một AI-First Business.

ERPNext giữ trạng thái và sự thật của doanh nghiệp. Custom Apps chứa nghiệp vụ. high_kode_integrations kết nối các hệ thống sản phẩm hiện hữu. high_kode_ai thực thi AI. AI Workforce thực hiện công việc theo Action được cấp phép. Human Governance kiểm soát rủi ro.

DingDongBot và AIWeb vẫn là SaaS độc lập, nhưng High Kode dùng hệ thống AI-First để quản trị hoạt động kinh doanh của chúng.

Khi High Kode vận hành được bằng hệ thống này, những workflow đã chứng minh mới được đóng gói thành AI Business Platform cho doanh nghiệp khác.

Build High Kode → AI vận hành High Kode → Prove → Productize → Scale.

Department | Action | Initial Level | Approval | Audit

Sales | Qualify Lead | L3 | No | Yes

Sales | Send Quotation | L2 | Yes | Yes

Support | Classify Ticket | L3 | No | Yes

Support | Draft Reply | L3 | Yes | Yes

Finance | Create Invoice | L2 | Yes | Yes

Finance | Refund | L1 | Yes | Yes

Marketing | Generate Content | L3 | Yes | Yes

Development | Create Code Task | L3 | No | Yes

Development | Production Deploy | L1 | Yes | Yes

Business area | Ví dụ AI Workforce

Sales | Sales Agent

Marketing | Marketing Agent

Development | Developer Agent

Projects | Project Agent

Finance | Finance Agent

HR | HR Agent

Support | Support Agent

Operations | Operations Agent

Metric | Ý nghĩa

Task Success Rate | Tỷ lệ task hoàn thành đúng

Human Hours Saved | Giờ người được giải phóng

Cost per Task | Chi phí AI/tool

Error Rate | Tỷ lệ hành động sai

Approval Rate | Tỷ lệ đề xuất được duyệt

Autonomous Task Ratio | Tỷ lệ task không cần người

Revenue/Cost Impact | Ảnh hưởng kinh doanh