# Values-Alignment
Values Alignment codes
# Universal Generosity Framework
## Adaptable Implementation Code for Any Application or Organization
### Framework Overview
This universal framework provides a standardized approach to implementing and measuring generosity across different applications, platforms, and organizational contexts. It includes modular components that can be adapted to various environments.
---
## Core Framework Components
### 1. Universal Generosity Interface
```python
from abc import ABC, abstractmethod
from typing import Dict, List, Any, Optional
from dataclasses import dataclass
from datetime import datetime, timedelta
import json

@dataclass
class GenerosityAction:
    """Universal representation of a generous action"""
    action_id: str
    actor_id: str
    recipient_id: Optional[str]
    action_type: str
    context: str
    timestamp: datetime
    impact_score: float
    metadata: Dict[str, Any]

@dataclass
class GenerosityMetric:
    """Universal metric structure"""
    metric_id: str
    name: str
    description: str
    value: float
    unit: str
    timestamp: datetime
    context: Dict[str, Any]

class GenerosityProvider(ABC):
    """Abstract interface for generosity implementations"""
    
    @abstractmethod
    def record_action(self, action: GenerosityAction) -> bool:
        """Record a generous action in the system"""
        pass
    
    @abstractmethod
    def get_metrics(self, entity_id: str, time_range: tuple) -> List[GenerosityMetric]:
        """Retrieve generosity metrics for an entity"""
        pass
    
    @abstractmethod
    def calculate_score(self, entity_id: str, time_range: tuple) -> float:
        """Calculate generosity score for an entity"""
        pass
    
    @abstractmethod
    def get_leaderboard(self, context: str, limit: int) -> List[Dict]:
        """Get generosity leaderboard for a context"""
        pass

class GenerosityFramework:
    """Universal generosity framework orchestrator"""
    
    def __init__(self, provider: GenerosityProvider, config: Dict[str, Any]):
        self.provider = provider
        self.config = config
        self.action_weights = config.get('action_weights', {})
        self.scoring_algorithm = config.get('scoring_algorithm', 'weighted_sum')
    
    def record_generous_action(self, 
                             actor_id: str,
                             action_type: str,
                             context: str,
                             recipient_id: Optional[str] = None,
                             metadata: Optional[Dict] = None) -> str:
        """Record a generous action with automatic scoring"""
        
        action = GenerosityAction(
            action_id=self._generate_id(),
            actor_id=actor_id,
            recipient_id=recipient_id,
            action_type=action_type,
            context=context,
            timestamp=datetime.now(),
            impact_score=self._calculate_impact_score(action_type, metadata or {}),
            metadata=metadata or {}
        )
        
        success = self.provider.record_action(action)
        
        if success:
            self._trigger_recognition(action)
            self._update_real_time_metrics(action)
        
        return action.action_id
    
    def get_generosity_profile(self, entity_id: str, days: int = 30) -> Dict:
        """Get comprehensive generosity profile for an entity"""
        
        end_date = datetime.now()
        start_date = end_date - timedelta(days=days)
        
        metrics = self.provider.get_metrics(entity_id, (start_date, end_date))
        score = self.provider.calculate_score(entity_id, (start_date, end_date))
        
        return {
            'entity_id': entity_id,
            'time_period': f"{days} days",
            'overall_score': score,
            'metrics': [metric.__dict__ for metric in metrics],
            'percentile_rank': self._calculate_percentile_rank(entity_id, score),
            'recommendations': self._generate_recommendations(entity_id, metrics),
            'trends': self._analyze_trends(entity_id, days)
        }
    
    def _calculate_impact_score(self, action_type: str, metadata: Dict) -> float:
        """Calculate impact score based on action type and context"""
        base_score = self.action_weights.get(action_type, 1.0)
        
        # Apply multipliers based on metadata
        multipliers = {
            'urgency': metadata.get('urgency', 1.0),
            'complexity': metadata.get('complexity', 1.0),
            'reach': metadata.get('reach', 1.0),
            'effort': metadata.get('effort', 1.0)
        }
        
        final_score = base_score * sum(multipliers.values()) / len(multipliers)
        return min(final_score, 10.0)  # Cap at 10
    
    def _generate_id(self) -> str:
        """Generate unique ID for actions"""
        import uuid
        return str(uuid.uuid4())
    
    def _trigger_recognition(self, action: GenerosityAction):
        """Trigger recognition mechanisms"""
        # Implement recognition logic based on configuration
        pass
    
    def _update_real_time_metrics(self, action: GenerosityAction):
        """Update real-time metrics after action"""
        # Implement real-time metric updates
        pass
    
    def _calculate_percentile_rank(self, entity_id: str, score: float) -> float:
        """Calculate percentile rank for entity"""
        # Implement percentile calculation
        return 0.0
    
    def _generate_recommendations(self, entity_id: str, metrics: List[GenerosityMetric]) -> List[str]:
        """Generate actionable recommendations"""
        # Implement recommendation engine
        return []
    
    def _analyze_trends(self, entity_id: str, days: int) -> Dict:
        """Analyze generosity trends"""
        # Implement trend analysis
        return {}
```

### 2. Configurable Action Types

```python
class GenerosityActionRegistry:
    """Registry for different types of generous actions"""
    
    def __init__(self):
        self.action_types = {}
        self._register_default_actions()
    
    def register_action_type(self, 
                           action_type: str,
                           name: str,
                           description: str,
                           base_score: float,
                           category: str,
                           validation_rules: Optional[Dict] = None):
        """Register a new action type"""
        
        self.action_types[action_type] = {
            'name': name,
            'description': description,
            'base_score': base_score,
            'category': category,
            'validation_rules': validation_rules or {},
            'created_at': datetime.now()
        }
    
    def _register_default_actions(self):
        """Register common generous actions"""
        
        default_actions = [
            # Knowledge Sharing
            ('knowledge_share', 'Knowledge Sharing', 'Sharing knowledge or expertise', 2.0, 'knowledge'),
            ('mentoring', 'Mentoring', 'Providing guidance and mentorship', 3.0, 'knowledge'),
            ('documentation', 'Documentation', 'Creating or improving documentation', 2.5, 'knowledge'),
            ('teaching', 'Teaching', 'Teaching or training others', 3.5, 'knowledge'),
            
            # Support and Help
            ('help_request_response', 'Help Response', 'Responding to help requests', 2.0, 'support'),
            ('problem_solving', 'Problem Solving', 'Helping solve problems', 2.5, 'support'),
            ('emergency_support', 'Emergency Support', 'Providing urgent support', 4.0, 'support'),
            ('technical_assistance', 'Technical Assistance', 'Providing technical help', 2.0, 'support'),
            
            # Recognition and Encouragement
            ('peer_recognition', 'Peer Recognition', 'Recognizing others\' contributions', 1.5, 'recognition'),
            ('encouragement', 'Encouragement', 'Encouraging team members', 1.0, 'recognition'),
            ('credit_attribution', 'Credit Attribution', 'Giving credit to others', 2.0, 'recognition'),
            ('celebration', 'Celebration', 'Celebrating others\' achievements', 1.5, 'recognition'),
            
            # Resource Sharing
            ('resource_sharing', 'Resource Sharing', 'Sharing tools, resources, or access', 2.5, 'resources'),
            ('opportunity_sharing', 'Opportunity Sharing', 'Sharing opportunities with others', 3.0, 'resources'),
            ('tool_creation', 'Tool Creation', 'Creating tools for others to use', 4.0, 'resources'),
            ('access_provision', 'Access Provision', 'Providing access to systems or information', 2.0, 'resources'),
            
            # Collaboration
            ('collaboration_initiation', 'Collaboration Initiation', 'Starting collaborative efforts', 2.0, 'collaboration'),
            ('cross_team_work', 'Cross-team Work', 'Working across team boundaries', 3.0, 'collaboration'),
            ('volunteer_work', 'Volunteer Work', 'Volunteering for additional tasks', 2.5, 'collaboration'),
            ('conflict_resolution', 'Conflict Resolution', 'Helping resolve conflicts', 3.5, 'collaboration')
        ]
        
        for action_type, name, description, base_score, category in default_actions:
            self.register_action_type(action_type, name, description, base_score, category)
    
    def get_action_info(self, action_type: str) -> Optional[Dict]:
        """Get information about an action type"""
        return self.action_types.get(action_type)
    
    def get_actions_by_category(self, category: str) -> List[Dict]:
        """Get all actions in a category"""
        return [
            {**action_info, 'action_type': action_type}
            for action_type, action_info in self.action_types.items()
            if action_info['category'] == category
        ]
```

### 3. Universal Metrics Calculator

```python
class UniversalMetricsCalculator:
    """Universal metrics calculation engine"""
    
    def __init__(self, config: Dict[str, Any]):
        self.config = config
        self.metric_definitions = self._load_metric_definitions()
    
    def calculate_all_metrics(self, 
                            entity_id: str, 
                            actions: List[GenerosityAction],
                            context: Dict[str, Any]) -> List[GenerosityMetric]:
        """Calculate all applicable metrics for an entity"""
        
        metrics = []
        
        for metric_id, definition in self.metric_definitions.items():
            try:
                value = self._calculate_metric(metric_id, definition, actions, context)
                
                metric = GenerosityMetric(
                    metric_id=metric_id,
                    name=definition['name'],
                    description=definition['description'],
                    value=value,
                    unit=definition['unit'],
                    timestamp=datetime.now(),
                    context=context
                )
                
                metrics.append(metric)
                
            except Exception as e:
                print(f"Error calculating metric {metric_id}: {e}")
                continue
        
        return metrics
    
    def _load_metric_definitions(self) -> Dict[str, Dict]:
        """Load metric definitions from configuration"""
        
        return {
            # Frequency Metrics
            'action_frequency': {
                'name': 'Action Frequency',
                'description': 'Number of generous actions per time period',
                'unit': 'actions/period',
                'calculation': 'count',
                'aggregation': 'sum'
            },
            
            'help_response_rate': {
                'name': 'Help Response Rate',
                'description': 'Percentage of help requests responded to',
                'unit': 'percentage',
                'calculation': 'ratio',
                'numerator_filter': {'action_type': 'help_request_response'},
                'denominator_source': 'help_requests_received'
            },
            
            'knowledge_sharing_frequency': {
                'name': 'Knowledge Sharing Frequency',
                'description': 'Frequency of knowledge sharing activities',
                'unit': 'actions/period',
                'calculation': 'count',
                'filter': {'category': 'knowledge'}
            },
            
            # Quality Metrics
            'average_impact_score': {
                'name': 'Average Impact Score',
                'description': 'Average impact score per action',
                'unit': 'score',
                'calculation': 'average',
                'field': 'impact_score'
            },
            
            'consistency_score': {
                'name': 'Consistency Score',
                'description': 'Consistency of generous behavior over time',
                'unit': 'score',
                'calculation': 'consistency',
                'time_window': 'weekly'
            },
            
            # Reach Metrics
            'recipient_diversity': {
                'name': 'Recipient Diversity',
                'description': 'Number of unique recipients helped',
                'unit': 'count',
                'calculation': 'unique_count',
                'field': 'recipient_id'
            },
            
            'cross_boundary_actions': {
                'name': 'Cross-boundary Actions',
                'description': 'Actions that cross organizational boundaries',
                'unit': 'count',
                'calculation': 'count',
                'filter': {'cross_boundary': True}
            },
            
            # Recognition Metrics
            'recognition_given': {
                'name': 'Recognition Given',
                'description': 'Amount of recognition given to others',
                'unit': 'count',
                'calculation': 'count',
                'filter': {'category': 'recognition'}
            },
            
            'credit_attribution_rate': {
                'name': 'Credit Attribution Rate',
                'description': 'Rate of attributing credit to others',
                'unit': 'percentage',
                'calculation': 'ratio',
                'numerator_filter': {'action_type': 'credit_attribution'},
                'denominator_source': 'collaborative_work_completed'
            }
        }
    
    def _calculate_metric(self, 
                         metric_id: str, 
                         definition: Dict, 
                         actions: List[GenerosityAction],
                         context: Dict) -> float:
        """Calculate a specific metric value"""
        
        calculation_type = definition['calculation']
        
        if calculation_type == 'count':
            return self._calculate_count(actions, definition.get('filter', {}))
        
        elif calculation_type == 'average':
            return self._calculate_average(actions, definition['field'], definition.get('filter', {}))
        
        elif calculation_type == 'ratio':
            numerator = self._calculate_count(actions, definition['numerator_filter'])
            denominator = context.get(definition['denominator_source'], 1)
            return (numerator / denominator) * 100 if denominator > 0 else 0
        
        elif calculation_type == 'unique_count':
            return self._calculate_unique_count(actions, definition['field'], definition.get('filter', {}))
        
        elif calculation_type == 'consistency':
            return self._calculate_consistency(actions, definition['time_window'])
        
        else:
            raise ValueError(f"Unknown calculation type: {calculation_type}")
    
    def _calculate_count(self, actions: List[GenerosityAction], filter_criteria: Dict) -> float:
        """Calculate count with filtering"""
        filtered_actions = self._filter_actions(actions, filter_criteria)
        return len(filtered_actions)
    
    def _calculate_average(self, actions: List[GenerosityAction], field: str, filter_criteria: Dict) -> float:
        """Calculate average of a field with filtering"""
        filtered_actions = self._filter_actions(actions, filter_criteria)
        
        if not filtered_actions:
            return 0.0
        
        values = [getattr(action, field) for action in filtered_actions if hasattr(action, field)]
        return sum(values) / len(values) if values else 0.0
    
    def _calculate_unique_count(self, actions: List[GenerosityAction], field: str, filter_criteria: Dict) -> float:
        """Calculate unique count of a field with filtering"""
        filtered_actions = self._filter_actions(actions, filter_criteria)
        
        unique_values = set()
        for action in filtered_actions:
            value = getattr(action, field, None)
            if value is not None:
                unique_values.add(value)
        
        return len(unique_values)
    
    def _calculate_consistency(self, actions: List[GenerosityAction], time_window: str) -> float:
        """Calculate consistency score based on time window"""
        # Group actions by time window and calculate consistency
        # Implementation depends on specific consistency algorithm
        return 0.0
    
    def _filter_actions(self, actions: List[GenerosityAction], criteria: Dict) -> List[GenerosityAction]:
        """Filter actions based on criteria"""
        filtered = []
        
        for action in actions:
            match = True
            
            for key, value in criteria.items():
                if key == 'category':
                    # Check metadata for category
                    if action.metadata.get('category') != value:
                        match = False
                        break
                elif key == 'action_type':
                    if action.action_type != value:
                        match = False
                        break
                elif hasattr(action, key):
                    if getattr(action, key) != value:
                        match = False
                        break
                elif key in action.metadata:
                    if action.metadata[key] != value:
                        match = False
                        break
            
            if match:
                filtered.append(action)
        
        return filtered
```

### 4. Application Adapters

```python
# GitHub Adapter
class GitHubGenerosityAdapter(GenerosityProvider):
    """Adapter for GitHub repositories"""
    
    def __init__(self, github_token: str, database_url: str):
        self.github_token = github_token
        self.db = self._connect_database(database_url)
        self.action_registry = GenerosityActionRegistry()
        
        # Register GitHub-specific actions
        self._register_github_actions()
    
    def _register_github_actions(self):
        """Register GitHub-specific generous actions"""
        github_actions = [
            ('code_review', 'Code Review', 'Providing code review feedback', 2.0, 'support'),
            ('issue_help', 'Issue Help', 'Helping with GitHub issues', 2.5, 'support'),
            ('documentation_pr', 'Documentation PR', 'Contributing documentation', 3.0, 'knowledge'),
            ('mentoring_comment', 'Mentoring Comment', 'Educational code comments', 2.0, 'knowledge'),
            ('cross_repo_contribution', 'Cross-repo Contribution', 'Contributing to other repositories', 3.5, 'collaboration')
        ]
        
        for action_type, name, description, base_score, category in github_actions:
            self.action_registry.register_action_type(action_type, name, description, base_score, category)
    
    def record_action(self, action: GenerosityAction) -> bool:
        """Record action in database with GitHub context"""
        try:
            query = """
            INSERT INTO generosity_actions 
            (action_id, actor_id, recipient_id, action_type, context, timestamp, impact_score, metadata)
            VALUES (?, ?, ?, ?, ?, ?, ?, ?)
            """
            
            self.db.execute(query, (
                action.action_id,
                action.actor_id,
                action.recipient_id,
                action.action_type,
                action.context,
                action.timestamp,
                action.impact_score,
                json.dumps(action.metadata)
            ))
            
            return True
        except Exception as e:
            print(f"Error recording action: {e}")
            return False
    
    def get_metrics(self, entity_id: str, time_range: tuple) -> List[GenerosityMetric]:
        """Get GitHub-specific metrics"""
        # Implementation for GitHub metrics
        return []
    
    def calculate_score(self, entity_id: str, time_range: tuple) -> float:
        """Calculate GitHub generosity score"""
        # Implementation for GitHub scoring
        return 0.0
    
    def get_leaderboard(self, context: str, limit: int) -> List[Dict]:
        """Get GitHub generosity leaderboard"""
        # Implementation for GitHub leaderboard
        return []

# Slack Adapter
class SlackGenerosityAdapter(GenerosityProvider):
    """Adapter for Slack workspaces"""
    
    def __init__(self, slack_token: str, database_url: str):
        self.slack_token = slack_token
        self.db = self._connect_database(database_url)
        self.action_registry = GenerosityActionRegistry()
        
        # Register Slack-specific actions
        self._register_slack_actions()
    
    def _register_slack_actions(self):
        """Register Slack-specific generous actions"""
        slack_actions = [
            ('helpful_response', 'Helpful Response', 'Providing helpful responses in channels', 1.5, 'support'),
            ('knowledge_thread', 'Knowledge Thread', 'Starting knowledge-sharing threads', 2.5, 'knowledge'),
            ('emoji_recognition', 'Emoji Recognition', 'Using emoji to recognize others', 0.5, 'recognition'),
            ('channel_help', 'Channel Help', 'Helping in public channels', 2.0, 'support'),
            ('dm_mentoring', 'DM Mentoring', 'Private mentoring conversations', 3.0, 'knowledge')
        ]
        
        for action_type, name, description, base_score, category in slack_actions:
            self.action_registry.register_action_type(action_type, name, description, base_score, category)

# Generic Web Application Adapter
class WebAppGenerosityAdapter(GenerosityProvider):
    """Generic adapter for web applications"""
    
    def __init__(self, database_url: str, custom_actions: Optional[List[Dict]] = None):
        self.db = self._connect_database(database_url)
        self.action_registry = GenerosityActionRegistry()
        
        # Register custom actions if provided
        if custom_actions:
            self._register_custom_actions(custom_actions)
    
    def _register_custom_actions(self, custom_actions: List[Dict]):
        """Register application-specific actions"""
        for action in custom_actions:
            self.action_registry.register_action_type(
                action['action_type'],
                action['name'],
                action['description'],
                action['base_score'],
                action['category']
            )
```

### 5. Configuration System

```yaml
# generosity_config.yaml
framework:
  name: "Universal Generosity Framework"
  version: "1.0.0"
  
# Scoring Configuration
scoring:
  algorithm: "weighted_sum"  # Options: weighted_sum, neural_network, rule_based
  time_decay_factor: 0.95    # How much recent actions are weighted
  max_score: 100             # Maximum possible score
  min_actions_for_score: 5   # Minimum actions needed for reliable scoring

# Action Weight Configuration
action_weights:
  # Knowledge Sharing
  knowledge_share: 2.0
  mentoring: 3.0
  documentation: 2.5
  teaching: 3.5
  
  # Support
  help_request_response: 2.0
  problem_solving: 2.5
  emergency_support: 4.0
  technical_assistance: 2.0
  
  # Recognition
  peer_recognition: 1.5
  encouragement: 1.0
  credit_attribution: 2.0
  celebration: 1.5
  
  # Resources
  resource_sharing: 2.5
  opportunity_sharing: 3.0
  tool_creation: 4.0
  access_provision: 2.0
  
  # Collaboration
  collaboration_initiation: 2.0
  cross_team_work: 3.0
  volunteer_work: 2.5
  conflict_resolution: 3.5

# Recognition Configuration
recognition:
  enabled: true
  
  triggers:
    score_milestones: [25, 50, 75, 100]
    action_streaks: [5, 10, 20, 30]
    category_mastery: ["knowledge", "support", "recognition", "resources"]
  
  channels:
    - type: "notification"
      config:
        immediate: true
        digest: "weekly"
    
    - type: "leaderboard"
      config:
        public: true
        anonymous_option: true
    
    - type: "badge_system"
      config:
        levels: ["Bronze", "Silver", "Gold", "Platinum"]

# Metrics Configuration
metrics:
  collection_frequency: "daily"
  retention_period: "2 years"
  
  dashboards:
    - name: "Individual Dashboard"
      metrics: ["action_frequency", "average_impact_score", "recipient_diversity"]
      refresh_rate: "hourly"
    
    - name: "Team Dashboard"
      metrics: ["team_collaboration_score", "knowledge_sharing_index", "support_responsiveness"]
      refresh_rate: "daily"
    
    - name: "Organization Dashboard" 
      metrics: ["organization_generosity_index", "cross_team_collaboration", "culture_health_score"]
      refresh_rate: "weekly"

# Integration Configuration
integrations:
  database:
    type: "postgresql"  # Options: postgresql, mysql, mongodb, sqlite
    connection_string: "${DATABASE_URL}"
    
  notification_systems:
    - type: "email"
      config:
        smtp_server: "${SMTP_SERVER}"
        from_address: "${FROM_EMAIL}"
    
    - type: "slack"
      config:
        webhook_url: "${SLACK_WEBHOOK_URL}"
        channel: "#generosity"
    
    - type: "teams"
      config:
        webhook_url: "${TEAMS_WEBHOOK_URL}"

# Privacy and Security
privacy:
  anonymization_enabled: true
  data_retention_days: 730
  opt_out_allowed: true
  
  sensitive_fields:
    - "recipient_id"
    - "personal_metadata"
  
  access_controls:
    individual_data: "self_only"
    aggregate_data: "team_leads"
    admin_functions: "hr_generosity_admins"

# Customization Options
customization:
  theme_colors:
    primary: "#4CAF50"
    secondary: "#2196F3"
    accent: "#FF9800"
  
  terminology:
    generosity: "Generosity"
    generous_action: "Generous Action"
    generosity_score: "Generosity Score"
    leaderboard: "Recognition Board"
  
  localization:
    default_language: "en"
    supported_languages: ["en", "es", "fr", "de", "zh"]
```

### 6. Implementation Examples

```python
# Example 1: E-commerce Platform Implementation
def setup_ecommerce_generosity():
    """Setup generosity tracking for e-commerce platform"""
    
    # Custom actions for e-commerce
    ecommerce_actions = [
        {
            'action_type': 'customer_help',
            'name': 'Customer Help',
            'description': 'Helping customers with issues',
            'base_score': 2.5,
            'category': 'support'
        },
        {
            'action_type': 'knowledge_base_contribution',
            'name': 'Knowledge Base Contribution',
            'description': 'Contributing to internal knowledge base',
            'base_score': 3.0,
            'category': 'knowledge'
        },
        {
            'action_type': 'cross_department_support',
            'name': 'Cross-department Support',
            'description': 'Supporting other departments',
            'base_score': 3.5,
            'category': 'collaboration'
        }
    ]
    
    # Initialize framework
    adapter = WebAppGenerosityAdapter(
        database_url="postgresql://localhost/ecommerce_generosity",
        custom_actions=ecommerce_actions
    )
    
    config = {
        'scoring_algorithm': 'weighted_sum',
        'action_weights': {
            'customer_help': 2.5,
            'knowledge_base_contribution': 3.0,
            'cross_department_support': 3.5
        }
    }
    
    framework = GenerosityFramework(adapter, config)
    return framework

# Example 2: Educational Platform Implementation
def setup_education_generosity():
    """Setup generosity tracking for educational platform"""
    
    education_actions = [
        {
            'action_type': 'student_tutoring',
            'name': 'Student Tutoring',
            'description': 'Tutoring other students',
            'base_score': 4.0,
            'category': 'knowledge'
        },
        {
            'action_type': 'resource_creation',
            'name': 'Resource Creation',
            'description': 'Creating educational resources',
            'base_score': 4.5,
            'category': 'resources'
        },
        {
            'action_type': 'peer_feedback',
            'name': 'Peer Feedback',
            'description': 'Providing constructive peer feedback',
            'base_score': 2.0,
            'category': 'support'
        }
    ]
    
    adapter = WebAppGenerosityAdapter(
        database_url="postgresql://localhost/education_generosity",
        custom_actions=education_actions
    )
    
    config = {
        'scoring_algorithm': 'weighted_sum',
        'action_weights': {
            'student_tutoring': 4.0,
            'resource_creation': 4.5,
            'peer_feedback': 2.0
        }
    }
    
    framework = GenerosityFramework(adapter, config)
    return framework
