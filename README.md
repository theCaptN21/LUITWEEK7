AWSTemplateFormatVersion: 2010-09-09
Description: AWS CloudFormation Template for Week 7 at LUIT
Parameters:
  VpcId:
    Type: 'AWS::EC2::VPC::Id'
    Description: VpcId of your existing Virtual Private Cloud (VPC)
    ConstraintDescription: must be the VPC Id of an existing Virtual Private Cloud.
  Subnets:
    Type: 'List<AWS::EC2::Subnet::Id>'
    Description: The list of SubnetIds in your Virtual Private Cloud (VPC)
    ConstraintDescription: >-
      must be a list of at least three existing subnets associated with at least
      three different availability zones. They should be residing in the
      selected Virtual Private Cloud.
  InstanceType:
    Description: WebServer EC2 instance type
    Type: String
    Default: t2.micro
    AllowedValues:
      - t1.micro
      - t2.nano
      - t2.micro
      - t2.small
      - t2.medium
      - t2.large
      - m1.small
      - m1.medium
      - m1.large
      - m1.xlarge
      - m2.xlarge
      - m2.2xlarge
      - m2.4xlarge
      - m3.medium
      - m3.large
      - m3.xlarge
      - m3.2xlarge
      - m4.large
      - m4.xlarge
      - m4.2xlarge
      - m4.4xlarge
      - m4.10xlarge
      - c1.medium
      - c1.xlarge
      - c3.large
      - c3.xlarge
      - c3.2xlarge
      - c3.4xlarge
      - c3.8xlarge
      - c4.large
      - c4.xlarge
      - c4.2xlarge
      - c4.4xlarge
      - c4.8xlarge
      - g2.2xlarge
      - g2.8xlarge
      - r3.large
      - r3.xlarge
      - r3.2xlarge
      - r3.4xlarge
      - r3.8xlarge
      - i2.xlarge
      - i2.2xlarge
      - i2.4xlarge
      - i2.8xlarge
      - d2.xlarge
      - d2.2xlarge
      - d2.4xlarge
      - d2.8xlarge
      - hi1.4xlarge
      - hs1.8xlarge
      - cr1.8xlarge
      - cc2.8xlarge
      - cg1.4xlarge
    ConstraintDescription: must be a valid EC2 instance type.
  OperatorEMail:
    Description: EMail address to notify if there are any scaling operations
    Type: String
    AllowedPattern: >-
      ([a-zA-Z0-9_\-\.]+)@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.)|(([a-zA-Z0-9\-]+\.)+))([a-zA-Z]{2,4}|[0-9]{1,3})(\]?)
    ConstraintDescription: must be a valid email address.
  KeyName:
    Description: The EC2 Key Pair to allow SSH access to the instances
    Type: 'AWS::EC2::KeyPair::KeyName'
    ConstraintDescription: must be the name of an existing EC2 KeyPair.
  SSHLocation:
    Description: The IP address range that can be used to SSH to the EC2 instances
    Type: String
    MinLength: '9'
    MaxLength: '18'
    Default: 0.0.0.0/0
    AllowedPattern: '(\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})/(\d{1,2})'
    ConstraintDescription: must be a valid IP CIDR range of the form x.x.x.x/x.
Mappings:
  Region2Examples:
    ap-east-1:
      Examples: 'https://s3-ap-east-1.amazonaws.com/cloudformation-examples-ap-east-1'
    ap-northeast-1:
      Examples: >-
        https://s3-ap-northeast-1.amazonaws.com/cloudformation-examples-ap-northeast-1
    ap-northeast-2:
      Examples: >-
        https://s3-ap-northeast-2.amazonaws.com/cloudformation-examples-ap-northeast-2
    ap-northeast-3:
      Examples: >-
        https://s3-ap-northeast-3.amazonaws.com/cloudformation-examples-ap-northeast-3
    ap-south-1:
      Examples: 'https://s3-ap-south-1.amazonaws.com/cloudformation-examples-ap-south-1'
    ap-southeast-1:
      Examples: >-
        https://s3-ap-southeast-1.amazonaws.com/cloudformation-examples-ap-southeast-1
    ap-southeast-2:
      Examples: >-
        https://s3-ap-southeast-2.amazonaws.com/cloudformation-examples-ap-southeast-2
    ca-central-1:
      Examples: >-
        https://s3-ca-central-1.amazonaws.com/cloudformation-examples-ca-central-1
    cn-north-1:
      Examples: >-
        https://s3.cn-north-1.amazonaws.com.cn/cloudformation-examples-cn-north-1
    cn-northwest-1:
      Examples: >-
        https://s3.cn-northwest-1.amazonaws.com.cn/cloudformation-examples-cn-northwest-1
    eu-central-1:
      Examples: >-
        https://s3-eu-central-1.amazonaws.com/cloudformation-examples-eu-central-1
    eu-north-1:
      Examples: 'https://s3-eu-north-1.amazonaws.com/cloudformation-examples-eu-north-1'
    eu-west-1:
      Examples: 'https://s3-eu-west-1.amazonaws.com/cloudformation-examples-eu-west-1'
    eu-west-2:
      Examples: 'https://s3-eu-west-2.amazonaws.com/cloudformation-examples-eu-west-2'
    eu-west-3:
      Examples: 'https://s3-eu-west-3.amazonaws.com/cloudformation-examples-eu-west-3'
    me-south-1:
      Examples: 'https://s3-me-south-1.amazonaws.com/cloudformation-examples-me-south-1'
    sa-east-1:
      Examples: 'https://s3-sa-east-1.amazonaws.com/cloudformation-examples-sa-east-1'
    us-east-1:
      Examples: 'https://s3.amazonaws.com/cloudformation-examples-us-east-1'
    us-east-2:
      Examples: 'https://s3-us-east-2.amazonaws.com/cloudformation-examples-us-east-2'
    us-west-1:
      Examples: 'https://s3-us-west-1.amazonaws.com/cloudformation-examples-us-west-1'
    us-west-2:
      Examples: 'https://s3-us-west-2.amazonaws.com/cloudformation-examples-us-west-2'
  AWSInstanceType2Arch:
    t1.micro:
      Arch: HVM64
    t2.nano:
      Arch: HVM64
    t2.micro:
      Arch: HVM64
    t2.small:
      Arch: HVM64
    t2.medium:
      Arch: HVM64
    t2.large:
      Arch: HVM64
    m1.small:
      Arch: HVM64
    m1.medium:
      Arch: HVM64
    m1.large:
      Arch: HVM64
    m1.xlarge:
      Arch: HVM64
    m2.xlarge:
      Arch: HVM64
    m2.2xlarge:
      Arch: HVM64
    m2.4xlarge:
      Arch: HVM64
    m3.medium:
      Arch: HVM64
    m3.large:
      Arch: HVM64
    m3.xlarge:
      Arch: HVM64
    m3.2xlarge:
      Arch: HVM64
    m4.large:
      Arch: HVM64
    m4.xlarge:
      Arch: HVM64
    m4.2xlarge:
      Arch: HVM64
    m4.4xlarge:
      Arch: HVM64
    m4.10xlarge:
      Arch: HVM64
    c1.medium:
      Arch: HVM64
    c1.xlarge:
      Arch: HVM64
    c3.large:
      Arch: HVM64
    c3.xlarge:
      Arch: HVM64
    c3.2xlarge:
      Arch: HVM64
    c3.4xlarge:
      Arch: HVM64
    c3.8xlarge:
      Arch: HVM64
    c4.large:
      Arch: HVM64
    c4.xlarge:
      Arch: HVM64
    c4.2xlarge:
      Arch: HVM64
    c4.4xlarge:
      Arch: HVM64
    c4.8xlarge:
      Arch: HVM64
    g2.2xlarge:
      Arch: HVMG2
    g2.8xlarge:
      Arch: HVMG2
    r3.large:
      Arch: HVM64
    r3.xlarge:
      Arch: HVM64
    r3.2xlarge:
      Arch: HVM64
    r3.4xlarge:
      Arch: HVM64
    r3.8xlarge:
      Arch: HVM64
    i2.xlarge:
      Arch: HVM64
    i2.2xlarge:
      Arch: HVM64
    i2.4xlarge:
      Arch: HVM64
    i2.8xlarge:
      Arch: HVM64
    d2.xlarge:
      Arch: HVM64
    d2.2xlarge:
      Arch: HVM64
    d2.4xlarge:
      Arch: HVM64
    d2.8xlarge:
      Arch: HVM64
    hi1.4xlarge:
      Arch: HVM64
    hs1.8xlarge:
      Arch: HVM64
    cr1.8xlarge:
      Arch: HVM64
    cc2.8xlarge:
      Arch: HVM64
  AWSInstanceType2NATArch:
    t1.micro:
      Arch: NATHVM64
    t2.nano:
      Arch: NATHVM64
    t2.micro:
      Arch: NATHVM64
    t2.small:
      Arch: NATHVM64
    t2.medium:
      Arch: NATHVM64
    t2.large:
      Arch: NATHVM64
    m1.small:
      Arch: NATHVM64
    m1.medium:
      Arch: NATHVM64
    m1.large:
      Arch: NATHVM64
    m1.xlarge:
      Arch: NATHVM64
    m2.xlarge:
      Arch: NATHVM64
    m2.2xlarge:
      Arch: NATHVM64
    m2.4xlarge:
      Arch: NATHVM64
    m3.medium:
      Arch: NATHVM64
    m3.large:
      Arch: NATHVM64
    m3.xlarge:
      Arch: NATHVM64
    m3.2xlarge:
      Arch: NATHVM64
    m4.large:
      Arch: NATHVM64
    m4.xlarge:
      Arch: NATHVM64
    m4.2xlarge:
      Arch: NATHVM64
    m4.4xlarge:
      Arch: NATHVM64
    m4.10xlarge:
      Arch: NATHVM64
    c1.medium:
      Arch: NATHVM64
    c1.xlarge:
      Arch: NATHVM64
    c3.large:
      Arch: NATHVM64
    c3.xlarge:
      Arch: NATHVM64
    c3.2xlarge:
      Arch: NATHVM64
    c3.4xlarge:
      Arch: NATHVM64
    c3.8xlarge:
      Arch: NATHVM64
    c4.large:
      Arch: NATHVM64
    c4.xlarge:
      Arch: NATHVM64
    c4.2xlarge:
      Arch: NATHVM64
    c4.4xlarge:
      Arch: NATHVM64
    c4.8xlarge:
      Arch: NATHVM64
    g2.2xlarge:
      Arch: NATHVMG2
    g2.8xlarge:
      Arch: NATHVMG2
    r3.large:
      Arch: NATHVM64
    r3.xlarge:
      Arch: NATHVM64
    r3.2xlarge:
      Arch: NATHVM64
    r3.4xlarge:
      Arch: NATHVM64
    r3.8xlarge:
      Arch: NATHVM64
    i2.xlarge:
      Arch: NATHVM64
    i2.2xlarge:
      Arch: NATHVM64
    i2.4xlarge:
      Arch: NATHVM64
    i2.8xlarge:
      Arch: NATHVM64
    d2.xlarge:
      Arch: NATHVM64
    d2.2xlarge:
      Arch: NATHVM64
    d2.4xlarge:
      Arch: NATHVM64
    d2.8xlarge:
      Arch: NATHVM64
    hi1.4xlarge:
      Arch: NATHVM64
    hs1.8xlarge:
      Arch: NATHVM64
    cr1.8xlarge:
      Arch: NATHVM64
    cc2.8xlarge:
      Arch: NATHVM64
  AWSRegionArch2AMI:
    af-south-1:
      HVM64: ami-064cc455f8a1ef504
      HVMG2: NOT_SUPPORTED
    ap-east-1:
      HVM64: ami-f85b1989
      HVMG2: NOT_SUPPORTED
    ap-northeast-1:
      HVM64: ami-0b2c2a754d5b4da22
      HVMG2: ami-09d0e0e099ecabba2
    ap-northeast-2:
      HVM64: ami-0493ab99920f410fc
      HVMG2: NOT_SUPPORTED
    ap-northeast-3:
      HVM64: ami-01344f6f63a4decc1
      HVMG2: NOT_SUPPORTED
    ap-south-1:
      HVM64: ami-03cfb5e1fb4fac428
      HVMG2: ami-0244c1d42815af84a
    ap-southeast-1:
      HVM64: ami-0ba35dc9caf73d1c7
      HVMG2: ami-0e46ce0d6a87dc979
    ap-southeast-2:
      HVM64: ami-0ae99b503e8694028
      HVMG2: ami-0c0ab057a101d8ff2
    ca-central-1:
      HVM64: ami-0803e21a2ec22f953
      HVMG2: NOT_SUPPORTED
    cn-north-1:
      HVM64: ami-07a3f215cc90c889c
      HVMG2: NOT_SUPPORTED
    cn-northwest-1:
      HVM64: ami-0a3b3b10f714a0ff4
      HVMG2: NOT_SUPPORTED
    eu-central-1:
      HVM64: ami-0474863011a7d1541
      HVMG2: ami-0aa1822e3eb913a11
    eu-north-1:
      HVM64: ami-0de4b8910494dba0f
      HVMG2: ami-32d55b4c
    eu-south-1:
      HVM64: ami-08427144fe9ebdef6
      HVMG2: NOT_SUPPORTED
    eu-west-1:
      HVM64: ami-015232c01a82b847b
      HVMG2: ami-0d5299b1c6112c3c7
    eu-west-2:
      HVM64: ami-0765d48d7e15beb93
      HVMG2: NOT_SUPPORTED
    eu-west-3:
      HVM64: ami-0caf07637eda19d9c
      HVMG2: NOT_SUPPORTED
    me-south-1:
      HVM64: ami-0744743d80915b497
      HVMG2: NOT_SUPPORTED
    sa-east-1:
      HVM64: ami-0a52e8a6018e92bb0
      HVMG2: NOT_SUPPORTED
    us-east-1:
      HVM64: ami-032930428bf1abbff
      HVMG2: ami-0aeb704d503081ea6
    us-east-2:
      HVM64: ami-027cab9a7bf0155df
      HVMG2: NOT_SUPPORTED
    us-west-1:
      HVM64: ami-088c153f74339f34c
      HVMG2: ami-0a7fc72dc0e51aa77
    us-west-2:
      HVM64: ami-01fee56b22f308154
      HVMG2: ami-0fe84a5b4563d8f27
Resources:
  NotificationTopic:
    Type: 'AWS::SNS::Topic'
    Properties:
      Subscription:
        - Endpoint: !Ref OperatorEMail
          Protocol: email
  WebServerGroup:
    Type: 'AWS::AutoScaling::AutoScalingGroup'
    Properties:
      VPCZoneIdentifier: !Ref Subnets
      LaunchConfigurationName: !Ref LaunchConfig
      MinSize: '2'
      MaxSize: '5'
      TargetGroupARNs:
        - !Ref ALBTargetGroup
      NotificationConfiguration:
        TopicARN: !Ref NotificationTopic
        NotificationTypes:
          - 'autoscaling:EC2_INSTANCE_LAUNCH'
          - 'autoscaling:EC2_INSTANCE_LAUNCH_ERROR'
          - 'autoscaling:EC2_INSTANCE_TERMINATE'
          - 'autoscaling:EC2_INSTANCE_TERMINATE_ERROR'
    CreationPolicy:
      ResourceSignal:
        Timeout: PT15M
        Count: '1'
    UpdatePolicy:
      AutoScalingRollingUpdate:
        MinInstancesInService: '1'
        MaxBatchSize: '1'
        PauseTime: PT15M
        WaitOnResourceSignals: 'true'
  LaunchConfig:
    Type: 'AWS::AutoScaling::LaunchConfiguration'
    Metadata:
      Comment: Install a simple application
      'AWS::CloudFormation::Init':
        config:
          packages:
            yum:
              httpd: []
          files:
            /var/www/html/index.html:
              content: !Join 
                - |+

                - - <img src="
                  - !FindInMap 
                    - Region2Examples
                    - !Ref 'AWS::Region'
                    - Examples
                  - /cloudformation_graphic.png" alt="AWS CloudFormation Logo"/>
                  - '<h1>Congratulations, Blue Team Is Killing it!.</h1>'
              mode: '000644'
              owner: root
              group: root
            /etc/cfn/cfn-hup.conf:
              content: !Join 
                - ''
                - - |
                    [main]
                  - stack=
                  - !Ref 'AWS::StackId'
                  - |+

                  - region=
                  - !Ref 'AWS::Region'
                  - |+

              mode: '000400'
              owner: root
              group: root
            /etc/cfn/hooks.d/cfn-auto-reloader.conf:
              content: !Join 
                - ''
                - - |
                    [cfn-auto-reloader-hook]
                  - |
                    triggers=post.update
                  - >
                    path=Resources.LaunchConfig.Metadata.AWS::CloudFormation::Init
                  - 'action=/opt/aws/bin/cfn-init -v '
                  - '         --stack '
                  - !Ref 'AWS::StackName'
                  - '         --resource LaunchConfig '
                  - '         --region '
                  - !Ref 'AWS::Region'
                  - |+

                  - |
                    runas=root
              mode: '000400'
              owner: root
              group: root
          services:
            sysvinit:
              httpd:
                enabled: 'true'
                ensureRunning: 'true'
              cfn-hup:
                enabled: 'true'
                ensureRunning: 'true'
                files:
                  - /etc/cfn/cfn-hup.conf
                  - /etc/cfn/hooks.d/cfn-auto-reloader.conf
    Properties:
      KeyName: !Ref KeyName
      ImageId: !FindInMap 
        - AWSRegionArch2AMI
        - !Ref 'AWS::Region'
        - !FindInMap 
          - AWSInstanceType2Arch
          - !Ref InstanceType
          - Arch
      SecurityGroups:
        - !Ref InstanceSecurityGroup
      InstanceType: !Ref InstanceType
      UserData: !Base64 
        'Fn::Join':
          - ''
          - - |
              #!/bin/bash -xe
            - |
              yum update -y aws-cfn-bootstrap
            - |
              yum update -y aws-cli
            - '/opt/aws/bin/cfn-init -v '
            - '         --stack '
            - !Ref 'AWS::StackName'
            - '         --resource LaunchConfig '
            - '         --region '
            - !Ref 'AWS::Region'
            - |+

            - '/opt/aws/bin/cfn-signal -e $? '
            - '         --stack '
            - !Ref 'AWS::StackName'
            - '         --resource WebServerGroup '
            - '         --region '
            - !Ref 'AWS::Region'
            - |+

  WebServerScaleUpPolicy:
    Type: 'AWS::AutoScaling::ScalingPolicy'
    Properties:
      AdjustmentType: ChangeInCapacity
      AutoScalingGroupName: !Ref WebServerGroup
      Cooldown: '60'
      ScalingAdjustment: '1'
  WebServerScaleDownPolicy:
    Type: 'AWS::AutoScaling::ScalingPolicy'
    Properties:
      AdjustmentType: ChangeInCapacity
      AutoScalingGroupName: !Ref WebServerGroup
      Cooldown: '60'
      ScalingAdjustment: '-1'
  CPUAlarmHigh:
    Type: 'AWS::CloudWatch::Alarm'
    Properties:
      AlarmDescription: Trigger An Alarm if the CPU usage goes higher than 50%
      MetricName: CPUUtilization
      Namespace: AWS/EC2
      Statistic: Average
      Period: '60'
      EvaluationPeriods: '2'
      Threshold: '50'
      AlarmActions:
        - !Ref WebServerScaleUpPolicy
      Dimensions:
        - Name: AutoScalingGroupName
          Value: !Ref WebServerGroup
      ComparisonOperator: GreaterThanThreshold
  CPUAlarmLow:
    Type: 'AWS::CloudWatch::Alarm'
    Properties:
      AlarmDescription: Scale-down if CPU < 70% for 10 minutes
      MetricName: CPUUtilization
      Namespace: AWS/EC2
      Statistic: Average
      Period: '60'
      EvaluationPeriods: '2'
      Threshold: '70'
      AlarmActions:
        - !Ref WebServerScaleDownPolicy
      Dimensions:
        - Name: AutoScalingGroupName
          Value: !Ref WebServerGroup
      ComparisonOperator: LessThanThreshold
  ApplicationLoadBalancer:
    Type: 'AWS::ElasticLoadBalancingV2::LoadBalancer'
    Properties:
      Subnets: !Ref Subnets
  ALBListener:
    Type: 'AWS::ElasticLoadBalancingV2::Listener'
    Properties:
      DefaultActions:
        - Type: forward
          TargetGroupArn: !Ref ALBTargetGroup
      LoadBalancerArn: !Ref ApplicationLoadBalancer
      Port: '80'
      Protocol: HTTP
  ALBTargetGroup:
    Type: 'AWS::ElasticLoadBalancingV2::TargetGroup'
    Properties:
      HealthCheckIntervalSeconds: 30
      HealthCheckTimeoutSeconds: 5
      HealthyThresholdCount: 3
      Port: 80
      Protocol: HTTP
      UnhealthyThresholdCount: 5
      VpcId: !Ref VpcId
  InstanceSecurityGroup:
    Type: 'AWS::EC2::SecurityGroup'
    Properties:
      GroupDescription: Enable SSH access and HTTP from the load balancer only
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: '22'
          ToPort: '22'
          CidrIp: !Ref SSHLocation
        - IpProtocol: tcp
          FromPort: '80'
          ToPort: '80'
          SourceSecurityGroupId: !Select 
            - 0
            - !GetAtt 
              - ApplicationLoadBalancer
              - SecurityGroups
      VpcId: !Ref VpcId
Outputs:
  URL:
    Description: The URL of the website
    Value: !Join 
      - ''
      - - 'http://'
        - !GetAtt 
          - ApplicationLoadBalancer
          - DNSName
Metadata:
  'AWS::CloudFormation::Designer': {}
