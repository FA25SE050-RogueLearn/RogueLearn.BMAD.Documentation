erDiagram
    learning_paths {
        uuid id PK ""
        text name ""
        text description ""
        text type ""
        uuid subject_id FK ""
        uuid curriculum_version_id FK ""
        timestamp created_at ""
    }

    quests {
        uuid id PK ""
        text title ""
        text description ""
        text type ""
        text difficulty ""
        int xp_reward ""
        text skill_tags ""
        uuid subject_id FK ""
        uuid created_by FK ""
        timestamp created_at ""
    }

    quest_steps {
        uuid id PK ""
        uuid quest_id FK ""
        int step_number ""
        text title ""
        text type ""
        json content ""
        json validation_criteria ""
    }

    learning_path_quests {
        uuid learning_path_id PK,FK ""
        uuid quest_id PK,FK ""
        int sequence_order ""
        bool is_mandatory ""
    }

    quest_assessments {
        uuid id PK ""
        uuid quest_id FK ""
        text type ""
        json configuration ""
        json passing_criteria ""
    }

    skill_trees {
        uuid id PK ""
        text name ""
        text description ""
        uuid curriculum_version_id FK ""
        timestamp created_at ""
    }

    skills {
        uuid id PK ""
        uuid skill_tree_id FK ""
        text name ""
        text description ""
        int experience_required ""
        text level ""
    }

    quest_skills {
        uuid quest_id PK,FK ""
        uuid skill_id PK,FK ""
        int experience_points ""
    }

    skill_dependencies {
        uuid skill_id PK,FK ""
        uuid prerequisite_skill_id PK,FK ""
    }

    user_quest_attempts {
        uuid id PK ""
        uuid auth_user_id FK ""
        uuid quest_id FK ""
        text status ""
        timestamp started_at ""
        timestamp completed_at ""
    }

    user_quest_step_progress {
        uuid id PK ""
        uuid attempt_id FK ""
        uuid step_id FK ""
        text status ""
        json submission_data ""
    }

    user_learning_path_progress {
        uuid id PK ""
        uuid auth_user_id FK ""
        uuid learning_path_id FK ""
        text status ""
    }
    
    learning_paths||--o{learning_path_quests:" "
    quests||--o{learning_path_quests:" "
    quests||--o{quest_steps:" "
    quests||--o{quest_assessments:" "
    quests||--o{quest_skills:" "
    skills||--o{quest_skills:" "
    skill_trees||--o{skills:" "
    skills||--o{skill_dependencies:" "
    skills||--o{skill_dependencies:" "
    quests||--o{user_quest_attempts:" "
    user_quest_attempts||--o{user_quest_step_progress:" "
    quest_steps||--o{user_quest_step_progress:" "
    learning_paths||--o{user_learning_path_progress:" "