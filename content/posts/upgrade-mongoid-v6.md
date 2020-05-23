---
title: 'Upgrade mongoid from v5.4.0 to v6.4.2'
date: 2019-04-01T17:17:02+09:00
draft: false
---

## Upgrading mongoid

* v5.4.0 -> v6.4.2
* refs:
 https://docs.mongodb.com/mongoid/6.4/tutorials/mongoid-upgrade/#upgrade-to-6-x-series

## 6.0.0.beta

https://github.com/mongodb/mongoid/releases/tag/v6.0.0.beta

* [MONGOID-3944](https://jira.mongodb.org/browse/MONGOID-3944) Support #create_with method.
  * 🆕#create_with メソッドの追加
  * ActiveRecord の #create_with の mongoid 版
  * https://qiita.com/pekepek/items/c57d4859e6e1ec8b1ad1#1-create_with
* [MONGOID-4019](https://jira.mongodb.org/browse/MONGOID-4019), [MONGOID-4182](https://jira.mongodb.org/browse/MONGOID-4182) Don't include
 Mongoi::Attributes::Dynamic when cloning documents with unknown attributes.
* [MONGOID-4167](https://jira.mongodb.org/browse/MONGOID-4167) Raise an exception if Model#with is called with invalid driver options.
  * `#with` メソッドに利用できないオプションを指定すると例外が発生するようになる
  * ⚠️
    古いオプションを指定している箇所があるので修正が必要 -> `with(safe: true)`

* [MONGOID-4264](https://jira.mongodb.org/browse/MONGOID-4264) Require belongs_to by default and provide options to override requirement.
  * ActiveRecord の belongs_to_required_by_default の mongoid 版
  * ⚠️ `belongs_to` 対象の id フィールド必須がデフォルトに
  * 📝 mongoid.yml の `belongs_to_required_by_default` の設定でデフォルト設定を変更可能

* [MONGOID-4193](https://jira.mongodb.org/browse/MONGOID-4193) Override attributes in criteria's selector with write method's attributes.
  * `#first_or_initialize` や `#find_or_create` の挙動が ActiveRecord と異なるのが修正された
  * :warning: qriteria (where etc) と組み合わせたときに初期値が qriteria になってしまうのが修正された

* [MONGOID-4270](https://jira.mongodb.org/browse/MONGOID-4270) Take out run_targeted_callbacks
  * ActiveSupport 4.0 サポート終了

* [MONGOID-4239 Validate field names.](https://jira.mongodb.org/browse/MONGOID-4239)
  * フィールド名に mongoid のメソッド名を指定することを禁止に
  * フィールド名チェックとそのエラーメッセージが実装された

## #with に許可されているオプション (MONGOID-4167)

```rb
Mongo::Client::VALID_OPTIONS + Mongoid::PersistenceContext::EXTRA_OPTIONS
=> [:app_name,
 :auth_mech,
 :auth_mech_properties,
 :auth_source,
 :connect,
 :connect_timeout,
 :compressors,
 :database,
 :heartbeat_frequency,
 :id_generator,
 :local_threshold,
 :logger,
 :max_idle_time,
 :max_pool_size,
 :max_read_retries,
 :min_pool_size,
 :monitoring,
 :password,
 :platform,
 :read,
 :read_concern,
 :read_retry_interval,
 :replica_set,
 :retry_writes,
 :server_selection_timeout,
 :socket_timeout,
 :ssl,
 :ssl_ca_cert,
 :ssl_ca_cert_string,
 :ssl_ca_cert_object,
 :ssl_cert,
 :ssl_cert_string,
 :ssl_cert_object,
 :ssl_key,
 :ssl_key_string,
 :ssl_key_object,
 :ssl_key_pass_phrase,
 :ssl_verify,
 :truncate_logs,
 :user,
 :wait_queue_timeout,
 :write,
 :zlib_compression_level,
 :client,
 :collection]
```

## first_or_initialize の挙動の変更 (MONGOID-4193)

```rb
User.where(name: 'ren').exists? # => false

u = User.where(name: 'ren').first_or_initialize(n: 'pitohui')

# mongoid 5.x
u.name # => ren

# mongoid 6.0.0
u.name # => pitohui
```

📝 `first_or_initialize` に渡すフィールド名にエイリアスは使えない

```rb
u = User.where(name: 'ren'),first_or_initialize(name: 'pitohui')
u.name # => ren

# 'pitohui' で初期化されない
```

## field, relation name に使えない名前 (MONGOID-4239)

```rb
# mongoid v6.1.1 の結果
Mongoid.destructive_fields
=> [:flagged_destroys,
 :add_atomic_pull,
 :add_atomic_unset,
 :delayed_atomic_sets,
 :atomic_path,
 :atomic_array_pushes,
 :atomic_array_pulls,
 :atomic_array_add_to_sets,
 :atomic_pulls,
 :delayed_atomic_pulls,
 :delayed_atomic_unsets,
 :atomic_attribute_name,
 :atomic_position,
 :atomic_updates,
 :process_flagged_destroys,
 :_updates,
 :atomic_delete_modifier,
 :atomic_paths,
 :atomic_insert_modifier,
 :flag_as_destroyed,
 :atomic_pushes,
 :atomic_sets,
 :atomic_unsets,
 :[],
 :[]=,
 :attributes,
 :attributes=,
 :has_attribute?,
 :read_attribute,
 :attribute_present?,
 :write_attribute,
 :remove_attribute,
 :assign_attributes,
 :raw_attributes,
 :attributes_before_type_cast,
 :has_attribute_before_type_cast?,
 :attribute_missing?,
 :read_attribute_before_type_cast,
 :write_attributes,
 :typed_attributes,
 :process_attributes,
 :clone,
 :dup,
 :changed,
 :changes,
 :changed?,
 :changed_attributes,
 :remove_change,
 :post_persist,
 :setters,
 :move_changes,
 :children_changed?,
 :previous_changes,
 :__evolve_object_id__,
 :attribute_names,
 :apply_defaults,
 :apply_pre_processed_defaults,
 :apply_post_processed_defaults,
 :using_object_ids?,
 :database_field_name,
 :apply_default,
 :lazy_settable?,
 :inspect,
 :run_callbacks,
 :run_before_callbacks,
 :run_after_callbacks,
 :callback_executable?,
 :in_callback_state?,
 :_matches?,
 :atomically,
 :fail_due_to_validation!,
 :fail_due_to_callback!,
 :hasherizer,
 :upsert,
 :update,
 :update_attribute,
 :update_attributes,
 :update!,
 :update_attributes!,
 :save,
 :save!,
 :positionally,
 :__metadata,
 :relation_metadata,
 :embedded?,
 :embedded_many?,
 :embedded_one?,
 :metadata_name,
 :referenced_many?,
 :referenced_one?,
 :reload_relations,
 :__metadata=,
 :reload,
 :serializable_hash,
 :shard_key_fields,
 :shard_key_selector,
 :persisted?,
 :new_record?,
 :destroyed?,
 :readonly?,
 :marked_for_destruction?,
 :flagged_for_destroy=,
 :new_record=,
 :destroyed=,
 :pushable?,
 :updateable?,
 :settable?,
 :flagged_for_destroy?,
 :cache_key,
 :_children,
 :_parent,
 :_root,
 :parentize,
 :_reset_memoized_children!,
 :collect_children,
 :remove_child,
 :hereditary?,
 :reset_persisted_children,
 :flag_children_persisted,
 :_parent=,
 :_root?,
 :valid?,
 :read_attribute_for_validation,
 :begin_validate,
 :exit_validate,
 :validated?,
 :performing_validations?,
 :validating_with_query?,
 :<=>,
 :==,
 :===,
 :eql?,
 :_synced,
 :_syncable?,
 :_synced?,
 :remove_inverse_keys,
 :update_inverse_keys,
 :persisted?,
 :assign_attributes,
 :sanitize_for_mass_assignment,
 :sanitize_forbidden_attributes,
 :valid?,
 :validate,
 :errors,
 :validate!,
 :read_attribute_for_validation,
 :invalid?,
 :validates_with,
 :run_validations!,
 :raise_validation_error,
 :==,
 :options,
 :collection,
 :client,
 :cluster,
 :database_name,
 :collection_name,
 :storage_options,
 :to_json,
 :`,
 :methods,
 :singleton_methods,
 :protected_methods,
 :private_methods,
 :public_methods,
 :to_yaml,
 :to_yaml_properties,
 :blank?,
 :presence,
 :present?,
 :psych_to_yaml,
 :as_json,
 :acts_like?,
 :to_param,
 :to_query,
 :deep_dup,
 :duplicable?,
 :in?,
 :presence_in,
 :instance_values,
 :instance_variable_names,
 :with_options,
 :html_safe?,
 :pry,
 :__binding__,
 :dclone,
 :require_dependency,
 :unloadable,
 :require_or_load,
 :load_dependency,
 :__array__,
 :__deep_copy__,
 :__add__,
 :__add_from_array__,
 :__intersect__,
 :__intersect_from_object__,
 :__intersect_from_array__,
 :__union__,
 :__union_from_object__,
 :__expand_complex__,
 :regexp?,
 :ivar,
 :__evolve_object_id__,
 :__find_args__,
 :__mongoize_object_id__,
 :__mongoize_time__,
 :blank_criteria?,
 :multi_arged?,
 :resizable?,
 :mongoize,
 :__to_inc__,
 :numeric?,
 :__sortable__,
 :__setter__,
 :do_or_do_not,
 :remove_ivar,
 :substitutable,
 :you_must,
 :to_bson_key,
 :to_bson_normalized_value,
 :to_bson_normalized_key,
 :pretty_print,
 :pretty_print_cycle,
 :pretty_print_instance_variables,
 :pretty_print_inspect,
 :to_ruby,
 :to_v8,
 :try,
 :try!,
 :tap,
 :public_send,
 :instance_variables,
 :instance_variable_set,
 :instance_variable_defined?,
 :remove_instance_variable,
 :kind_of?,
 :is_a?,
 :instance_variable_get,
 :instance_of?,
 :class_eval,
 :public_method,
 :extend,
 :define_singleton_method,
 :singleton_method,
 :to_enum,
 :enum_for,
 :suppress_warnings,
 :<=>,
 :===,
 :=~,
 :!~,
 :eql?,
 :respond_to?,
 :freeze,
 :inspect,
 :display,
 :ai,
 :object_id,
 :send,
 :to_s,
 :gem,
 :pretty_inspect,
 :awesome_inspect,
 :awesome_print,
 :byebug,
 :debugger,
 :method,
 :nil?,
 :hash,
 :class,
 :singleton_class,
 :clone,
 :dup,
 :itself,
 :taint,
 :tainted?,
 :untaint,
 :untrust,
 :trust,
 :untrusted?,
 :frozen?,
 :!,
 :!=,
 :__send__,
 :equal?,
 :instance_eval,
 :instance_exec,
 :__id__,
 :fields,
 :aliased_fields,
 :localized_fields,
 :index_specifications,
 :shard_key_fields,
 :nested_attributes,
 :readonly_attributes,
 :storage_options,
 :cascades,
 :cyclic,
 :cache_timestamp_format]
```

v6.1.0 で Mongoid::PersistenceContext のインスタンスメソッドも予約語に追加されたことで予約語が２倍に
https://github.com/mongodb/mongoid/blob/6.1.0-stable/lib/mongoid/composable.rb#L88

v6.4.0 では Mongoid::Clients::Options が追加されており、以下が予約語に追加されている
`[:with, :collection, :collection_name, :persistence_context, :mongo_client]`

## 6.0.0.rc0

https://github.com/mongodb/mongoid/releases/tag/v6.0.0.rc0

* [MONGOID-4052](https://jira.mongodb.org/browse/MONGOID-4052) #only and #without methods are not scoped to specific locales, unless user explicitly does so.
  * `#only` , `#without` メソッドに localize オプションが追加
  * https://github.com/mongodb/mongoid/pull/4277
* Criteria#map uses `#pluck`, not `#only`

## 6.0.0

https://github.com/mongodb/mongoid/releases/tag/v6.0.0

* MONGOID-4173 [https://jira.mongodb.org/browse/MONGOID-4173] Fix nested eager loading. * v5.1.0 でも修正済み

```rb
article = Article.includes(user: [:blog]).first
MONGODB | localhost:27017 | blog_development.find | STARTED | {"find"=>"articles", "filter"=>{}, "limit"=>1, "singleBatch"=>true}
MONGODB | localhost:27017 | blog_development.find | STARTED | {"find"=>"users", "filter"=>{"_id"=>{"$in"=>[BSON::ObjectId('5c198c15611e57086a751514')]}}}
MONGODB | localhost:27017 | blog_development.find | STARTED | {"find"=>"blogs", "filter"=>{"_id"=>{"$in"=>[BSON::ObjectId('5c198c14611e57086a751510')]}}}
```
