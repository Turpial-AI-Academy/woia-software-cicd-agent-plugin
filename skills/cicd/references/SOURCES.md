# Reference Sources

Use these sources when current external verification is useful.

## Agent Plugin and Skill formats

- Agent Plugins specification: https://agent-plugins.org/specification
- Agent Plugins 1.0.0 manifest schema: https://agent-plugins.org/schemas/1.0.0/plugin.schema.json
- Agent Plugins schema source repository: https://github.com/agentplugins/agent-plugins-spec/blob/main/schemas/1.0.0/plugin.schema.json
- Agent Plugins author guide: https://agent-plugins.org/plugin-authors/build-an-agent-plugin
- Agent Plugins manifest: https://agent-plugins.org/plugin-authors/manifest
- Agent Plugins skills: https://agent-plugins.org/plugin-authors/skills
- Agent Skills specification: https://agentskills.io/specification
- Agent Skills reference validator (`skills-ref`): https://github.com/agentskills/agentskills/tree/main/skills-ref

The repository validates its own manifest against a versioned offline copy of the Agent Plugins 1.0.0 schema. It parses skill frontmatter as YAML and checks the current Agent Skills fields. `skills-ref` is the specification's reference library and can provide an independent check; it is documented as a demonstration implementation, not a runtime dependency.

## CI/CD and delivery

- DORA: https://dora.dev/
- DORA Continuous Delivery capability: https://dora.dev/capabilities/continuous-delivery/
- Martin Fowler, Deployment Pipeline: https://martinfowler.com/bliki/DeploymentPipeline.html
- Martin Fowler, Test Pyramid: https://martinfowler.com/bliki/TestPyramid.html
- Trunk-Based Development: https://trunkbaseddevelopment.com/

## Remote CI provider references

- Workflow syntax: https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax
- Dependency caching: https://docs.github.com/en/actions/reference/workflows-and-actions/dependency-caching
- Reusable workflows: https://docs.github.com/en/actions/concepts/workflows-and-actions/reusing-workflow-configurations
- Protected branches: https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches
- Secure use: https://docs.github.com/en/actions/reference/security/secure-use

## Supply chain

- OpenSSF: https://openssf.org/

Load provider documentation when the repository's selected policy or delivery model uses that provider. These references do not select a provider. When a practice has changed or provider behavior is time-sensitive, verify the current official documentation rather than relying solely on this bundled snapshot.

## Plugin packaging references

- Agent Plugins Specification 1.0.0: https://agent-plugins.org/specification
- OpenAI — Package your plugin: https://developers.openai.com/plugins/build/plugins
- OpenAI — Build skills: https://developers.openai.com/plugins/build/skills
