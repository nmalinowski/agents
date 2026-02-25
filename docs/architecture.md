# Architecture & Design Principles

This marketplace follows industry best practices with a focus on granularity, composability, and minimal token usage.

## Core Philosophy

### Single Responsibility Principle

- Each plugin does **one thing well** (Unix philosophy)
- Clear, focused purposes (describable in 5-10 words)
- Average plugin size: **3.4 components** (follows Anthropic's 2-8 pattern)
- **Zero bloated plugins** - all plugins focused and purposeful

### Composability Over Bundling

- Mix and match plugins based on needs
- Workflow orchestrators compose focused plugins
- No forced feature bundling
- Clear boundaries between plugins

### Context Efficiency

- Smaller tools = faster processing
- Better fit in LLM context windows
- More accurate, focused responses
- Install only what you need

### Maintainability

- Single-purpose = easier updates
- Clear boundaries = isolated changes
- Less duplication = simpler maintenance
- Isolated dependencies

## Granular Plugin Architecture

### Plugin Distribution

- **67 focused plugins** optimized for specific use cases
- **23 clear categories** with 1-6 plugins each for easy discovery
- Organized by domain:
  - **Development**: 4 plugins (debugging, backend, frontend, multi-platform)
  - **Security**: 4 plugins (scanning, compliance, backend-api, frontend-mobile)
  - **Operations**: 4 plugins (incident, diagnostics, distributed, observability)
  - **Languages**: 7 plugins (Python, JS/TS, systems, JVM, scripting, functional, embedded)
  - **Infrastructure**: 5 plugins (deployment, validation, K8s, cloud, CI/CD)
  - And 18 more specialized categories

### Component Breakdown

**99 Specialized Agents**

- Domain experts with deep knowledge
- Organized across architecture, languages, infrastructure, quality, data/AI, documentation, business, and SEO
- Model-optimized with three-tier strategy (Opus, Sonnet, Haiku) for performance and cost

**15 Workflow Orchestrators**

- Multi-agent coordination systems
- Complex operations like full-stack development, security hardening, ML pipelines, incident response
- Pre-configured agent workflows

**71 Development Tools**

- Optimized utilities including:
  - Project scaffolding (Python, TypeScript, Rust)
  - Security scanning (SAST, dependency audit, XSS)
  - Test generation (pytest, Jest)
  - Component scaffolding (React, React Native)
  - Infrastructure setup (Terraform, Kubernetes)

**107 Agent Skills**

- Modular knowledge packages
- Progressive disclosure architecture
- Domain-specific expertise across 18 plugins
- Spec-compliant (Anthropic Agent Skills Specification)

## Repository Structure

```
claude-agents/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace catalog - Claude Code (backward compat)
├── .github/
│   └── plugin/
│       └── marketplace.json      # Marketplace catalog - Copilot CLI
├── plugins/                       # Isolated plugin directories
│   ├── python-development/
│   │   ├── agents/               # Python language agents
│   │   │   ├── python-pro.agent.md
│   │   │   ├── django-pro.agent.md
│   │   │   └── fastapi-pro.agent.md
│   │   ├── commands/             # Python tooling
│   │   │   └── python-scaffold.md
│   │   └── skills/               # Python skills (5 total)
│   │       ├── async-python-patterns/
│   │       ├── python-testing-patterns/
│   │       ├── python-packaging/
│   │       ├── python-performance-optimization/
│   │       └── uv-package-manager/
│   ├── backend-development/
│   │   ├── agents/
│   │   │   ├── backend-architect.agent.md
│   │   │   ├── graphql-architect.agent.md
│   │   │   └── tdd-orchestrator.agent.md
│   │   ├── commands/
│   │   │   └── feature-development.md
│   │   └── skills/               # Backend skills (3 total)
│   │       ├── api-design-principles/
│   │       ├── architecture-patterns/
│   │       └── microservices-patterns/
│   ├── security-scanning/
│   │   ├── agents/
│   │   │   └── security-auditor.agent.md
│   │   ├── commands/
│   │   │   ├── security-hardening.md
│   │   │   ├── security-sast.md
│   │   │   └── security-dependencies.md
│   │   └── skills/               # Security skills (1 total)
│   │       └── sast-configuration/
│   ├── c4-architecture/
│   │   ├── agents/               # C4 architecture agents
│   │   │   ├── c4-code.agent.md
│   │   │   ├── c4-component.agent.md
│   │   │   ├── c4-container.agent.md
│   │   │   └── c4-context.agent.md
│   │   └── commands/
│   │       └── c4-architecture.md
│   └── ... (62 more isolated plugins)
├── docs/                          # Documentation
│   ├── agent-skills.md           # Agent Skills guide
│   ├── agents.md                 # Agent reference
│   ├── plugins.md                # Plugin catalog
│   ├── usage.md                  # Usage guide
│   └── architecture.md           # This file
└── README.md                      # Quick start
```

## Dual-Compatibility Architecture

This repository supports both **Claude Code** and **GitHub Copilot CLI** as agent hosts:

- **`.claude-plugin/marketplace.json`** — Original catalog location for Claude Code (maintained for backward compatibility)
- **`.github/plugin/marketplace.json`** — Catalog location for Copilot CLI discovery
- Both files share the same schema; `plugin.json` includes explicit component path fields (`agents`, `skills`, `commands`) so each host can resolve plugin contents unambiguously
- Agent files use the `.agent.md` extension (e.g., `python-pro.agent.md`) to distinguish them from plain documentation Markdown

## Plugin Structure

Each plugin contains:

- **agents/** - Specialized agents for that domain, using `.agent.md` extension (optional)
- **commands/** - Tools and workflows specific to that plugin (optional)
- **skills/** - Modular knowledge packages with progressive disclosure (optional)

### Minimum Requirements

- At least one agent OR one command
- Clear, focused purpose
- Proper frontmatter in all files
- Entry in marketplace.json

### Example Plugin

```
plugins/kubernetes-operations/
├── agents/
│   └── kubernetes-architect.agent.md   # K8s architecture and design
├── commands/
│   └── k8s-deploy.md                  # Deployment automation
└── skills/
    ├── k8s-manifest-generator/         # Manifest creation skill
    ├── helm-chart-scaffolding/         # Helm chart skill
    ├── gitops-workflow/                # GitOps automation skill
    └── k8s-security-policies/          # Security policy skill
```

## Agent Skills Architecture

### Progressive Disclosure

Skills use a three-tier architecture for token efficiency:

1. **Metadata** (Frontmatter): Name and activation criteria (always loaded)
2. **Instructions**: Core guidance and patterns (loaded when activated)
3. **Resources**: Examples and templates (loaded on demand)

### Specification Compliance

All skills follow the [Agent Skills Specification](https://github.com/anthropics/skills/blob/main/agent_skills_spec.md):

```yaml
---
name: skill-name # Required: hyphen-case
description: What the skill does. Use when [trigger]. # Required: < 1024 chars
---
# Skill content with progressive disclosure
```

### Benefits

- **Token Efficiency**: Load only relevant knowledge when needed
- **Specialized Expertise**: Deep domain knowledge without bloat
- **Clear Activation**: Explicit triggers prevent unwanted invocation
- **Composability**: Mix and match skills across workflows
- **Maintainability**: Isolated updates don't affect other skills

See [Agent Skills](./agent-skills.md) for complete details on the 107 skills.

## Model Configuration Strategy

### Two-Tier Architecture

The system uses Claude Opus and Sonnet models strategically:

| Model  | Count     | Use Case                                     |
| ------ | --------- | -------------------------------------------- |
| Opus   | 42 agents | Critical architecture, security, code review |
| Sonnet | 39 agents | Complex tasks, support with intelligence     |
| Haiku  | 18 agents | Fast operational tasks                       |

> **Model field convention:** Agent frontmatter uses full Copilot CLI model names
> (e.g., `claude-opus-4.6`, `claude-sonnet-4.5`, `claude-haiku-4.5`) instead of
> short aliases like `opus` or `sonnet`. This ensures compatibility with both
> Claude Code and GitHub Copilot CLI.

### Selection Criteria

**Haiku - Fast Execution & Deterministic Tasks**

- Generating code from well-defined specifications
- Creating tests following established patterns
- Writing documentation with clear templates
- Executing infrastructure operations
- Performing database query optimization
- Handling customer support responses
- Processing SEO optimization tasks
- Managing deployment pipelines

**Sonnet - Complex Reasoning & Architecture**

- Designing system architecture
- Making technology selection decisions
- Performing security audits
- Reviewing code for architectural patterns
- Creating complex AI/ML pipelines
- Providing language-specific expertise
- Orchestrating multi-agent workflows
- Handling business-critical legal/HR matters

### Hybrid Orchestration

Combine models for optimal performance and cost:

```
Planning Phase (Sonnet) → Execution Phase (Haiku) → Review Phase (Sonnet)

Example:
backend-architect (Sonnet) designs API
  ↓
Generate endpoints (Haiku) implements spec
  ↓
test-automator (Haiku) creates tests
  ↓
code-reviewer (Sonnet) validates architecture
```

## Performance & Quality

### Optimized Token Usage

- **Isolated plugins** load only what you need
- **Granular architecture** reduces unnecessary context
- **Progressive disclosure** (skills) loads knowledge on demand
- **Clear boundaries** prevent context pollution

### Component Coverage

- **100% agent coverage** - all plugins include at least one agent
- **100% component availability** - all 99 agents accessible across plugins
- **Efficient distribution** - 3.4 components per plugin average

### Discoverability

- **Clear plugin names** convey purpose immediately
- **Logical categorization** with 23 well-defined categories
- **Searchable documentation** with cross-references
- **Easy to find** the right tool for the job

## Design Patterns

### Pattern 1: Single-Purpose Plugin

Each plugin focuses on one domain:

```
python-development/
├── agents/           # Python language experts
├── commands/         # Python project scaffolding
└── skills/           # Python-specific knowledge
```

**Benefits:**

- Clear responsibility
- Easy to maintain
- Minimal token usage
- Composable with other plugins

### Pattern 2: Workflow Orchestration

Orchestrator plugins coordinate multiple agents:

```
full-stack-orchestration/
└── commands/
    └── full-stack-feature.md    # Coordinates 7+ agents
```

**Orchestration:**

1. backend-architect (design API)
2. database-architect (design schema)
3. frontend-developer (build UI)
4. test-automator (create tests)
5. security-auditor (security review)
6. deployment-engineer (CI/CD)
7. observability-engineer (monitoring)

### Pattern 3: Agent + Skill Integration

Agents provide reasoning, skills provide knowledge:

```
User: "Build FastAPI project with async patterns"
  ↓
fastapi-pro agent (orchestrates)
  ↓
fastapi-templates skill (provides patterns)
  ↓
python-scaffold command (generates project)
```

### Pattern 4: Multi-Plugin Composition

Complex workflows use multiple plugins:

```
Feature Development Workflow:
1. backend-development:feature-development
2. security-scanning:security-hardening
3. unit-testing:test-generate
4. comprehensive-review:full-review
5. cicd-automation:workflow-automate
6. observability-monitoring:monitor-setup
```

## Versioning & Updates

### Marketplace Updates

- Marketplace catalog in `.claude-plugin/marketplace.json` and `.github/plugin/marketplace.json`
- Semantic versioning for plugins
- Backward compatibility maintained
- Clear migration guides for breaking changes

### Plugin Updates

- Individual plugin updates don't affect others
- Skills can be updated independently
- Agents can be added/removed without breaking workflows
- Commands maintain stable interfaces

## Contributing Guidelines

### Adding a Plugin

1. Create plugin directory: `plugins/{plugin-name}/`
2. Add agents and/or commands
3. Optionally add skills
4. Update marketplace.json
5. Document in appropriate category

### Adding an Agent

1. Create `plugins/{plugin-name}/agents/{agent-name}.agent.md`
2. Add frontmatter (name, description, model)
3. Write comprehensive system prompt
4. Update plugin definition

### Adding a Skill

1. Create `plugins/{plugin-name}/skills/{skill-name}/SKILL.md`
2. Add YAML frontmatter (name, description with "Use when")
3. Write skill content with progressive disclosure
4. Add to plugin's skills array in marketplace.json

### Quality Standards

- **Clear naming** - Hyphen-case, descriptive
- **Focused scope** - Single responsibility
- **Complete documentation** - What, when, how
- **Tested functionality** - Verify before committing
- **Spec compliance** - Follow Anthropic guidelines

## See Also

- [Agent Skills](./agent-skills.md) - Modular knowledge packages
- [Agent Reference](./agents.md) - Complete agent catalog
- [Plugin Reference](./plugins.md) - All 67 plugins
- [Usage Guide](./usage.md) - Commands and workflows
