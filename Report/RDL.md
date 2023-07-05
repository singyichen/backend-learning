---
title: RDL
description: 
published: true
date: 2023-01-30T00:42:55.195Z
tags: 
editor: markdown
dateCreated: 2023-01-16T02:35:46.726Z
---

# RDL
- [ ] [報表定義語言 (SSRS)](https://learn.microsoft.com/zh-cn/sql/reporting-services/reports/report-definition-language-ssrs?view=sql-server-ver16)
- [ ] []()

# 報表定義語言 ( SSRS )
報表定義語言 ( RDL ) 是 SQL Server Reporting Services 報表定義的 XML 表示形式。 報表定義包含報表的資料檢索和佈局資訊。 RDL 由 XML 元素組成，這些元素符合為 Reporting Services 建立的 XML 語法。 通過訪問報表定義檔案中的程式碼程序集，可以新增您自己的自訂函數，以便控制報表項值、樣式和格式。

RDL 通過定義支援報表定義互換的公共架構，提升了商業報表產品的互操作性。 使用 XML 的任何協議或程式設計介面都可以使用 RDL。 RDL 是：

- 報表定義的 XML 架構。
- 企業和第三方的交換格式。
- 支援其他命名空間和自訂元素的可擴展開放式架構。

# RDL XML 架構定義
SQL Server Reporting Services 報表定義語言 ( RDL ) 檔案使用 XML 架構定義 ( XSD ) 檔案進行驗證。 架構定義 RDL 元素可在 .rdl 檔案中什麼位置出現的規則。 元素包括其資料類型和基數，即允許的出現次數。 元素可以是簡單的，也可以是複雜的。 簡單元素沒有子元素或屬性。 複雜元素具有子元素以及可選具有屬性。

例如，此架構包含 RDL 元素 ReportParameters，它為複雜類型 ReportParametersType。 根據約定，元素的複雜類型是元素名稱後跟單詞 Type。 ReportParameters 元素可包含在 報表 元素（複雜類型）中，並可包含 ReportParameter 元素。 ReportParameterType 是只能為下列值之一的簡單類型： Boolean、 DateTime、 Integer、 Float 或 String。 有關 XML 架構資料類型的詳細資訊，請參閱 XML Schema Part 2: Datatypes Second Edition（XML 架構第 2 部分：資料類型第二版）。

可在 ReportDefinition.xsd 檔案中找到 RDL XSD，該檔案位於產品 CD-ROM 的 Extras 資料夾中。 此外，還可通過以下 URL 在報表伺服器上獲取：https://servername/reportserver/reportdefinition.xsd 。

# RDL 類型
下表列出了在 RDL 元素和屬性中使用的類型。

|**類型**|**說明**|
|:-----:|:------:|
|**二進制**|具有 Base-64 編碼二進制值的屬性。|
|**布林值**|以 `true` 或 `false` 作為對象值的屬性。 除非另行指定，否則未指定的可選布林值對象的值為 False。|
|**日期**|具有以 ISO8601 日期格式 `YYYY-MM-DD[THH:MM[:SS[.S]]]` 指定的完全指定日期或日期時間值的屬性。|
|**Enum**|具有字串文字值的屬性，該文字值必須是指定值列表中的某個值。|
|**Float**|具有浮點值的屬性。 使用句號 (`.`) 作為可選的小數分隔符。|
|**整數**|具有整數 (int32) 值的屬性。|
|**語言**|具有包含語言和區域性程式碼（例如 "en-us" 表示"美國英語"）的文字值的屬性。 該值必須是特定語言，或者是在 Microsoft .NET Framework 中為其定義了默認語言的非特定語言。|
|**名稱**|具有字串文字值的屬性。 名稱在該項的命名空間中必須唯一。 如果未指定，則項的命名空間為具有名稱的最內層包含對象。|
|**NormalizedString**|具有已規範化的字串文字值的屬性。|
|**大小**|大小元素必須包含數字（以句點字元作為可選的小數分隔符）。 數字後面必須是 CSS 長度單位（例如 cm、mm、in、pt 或 pc）的指示符。 數字和指示符之間的空格是可選的。 有關大小指示符的詳細資訊，請參閱 CSS 值和單位參考。在 RDL 中， Size 的最大值為 160 in。 最小大小為 0 in。|
|**字串**|具有字串文字值的屬性。|
|**UnsignedInt**|具有無符號整數 (uint32) 值的屬性。|
|**變數**|具有任何簡單 XML 類型的屬性。|

# RDL 資料類型
DataType 列舉定義 RDL 中的屬性、表示式或參數的資料類型。 下表顯示公共語言執行階段 ( CLR ) 資料類型是如何與 RDL 資料類型相對應的。

|**RDL 相應的資料類型**|**CLR 類型**|
|:-------:|:----------------:|
|布林值|布林值|
|DateTime|DateTime、DateTimeOffset|
|Integer|Int16、Int32、UInt16、Byte、SByte|
|Float|Single、Double|
|字串|String、Char、GUID、Timespan|

# RDL 範例
```xml
<?xml version="1.0"?>
<Report xmlns:df="http://schemas.microsoft.com/sqlserver/reporting/2016/01/reportdefinition/defaultfontfamily" xmlns:rd="http://schemas.microsoft.com/SQLServer/reporting/reportdesigner" xmlns="http://schemas.microsoft.com/sqlserver/reporting/2016/01/reportdefinition">
  <ReportSections>
    <ReportSection>
      <Body>
        <Style>
          <FontFamily>Arial</FontFamily>
          <Border>
            <Style>None</Style>
          </Border>
        </Style>
        <ReportItems>
          <Tablix Name="Tablix1">
            <Left>42.75pt</Left>
            <Top>45pt</Top>
            <Height>103.5pt</Height>
            <Width>324pt</Width>
            <Style>
              <FontFamily>Arial</FontFamily>
              <Border>
                <Style>None</Style>
              </Border>
            </Style>
            <DataSetName>DataSet1</DataSetName>
            <TablixBody>
              <TablixColumns>
                <TablixColumn>
                  <Width>108pt</Width>
                </TablixColumn>
                <TablixColumn>
                  <Width>108pt</Width>
                </TablixColumn>
                <TablixColumn>
                  <Width>108pt</Width>
                </TablixColumn>
              </TablixColumns>
              <TablixRows>
                <TablixRow>
                  <Height>51.75pt</Height>
                  <TablixCells>
                    <TablixCell>
                      <CellContents>
                        <Textbox Name="TextBox9">
                          <Left>0in</Left>
                          <Top>0in</Top>
                          <Height>51.75pt</Height>
                          <Width>108pt</Width>
                          <Style>
                            <FontFamily>Arial</FontFamily>
                            <PaddingLeft>2pt</PaddingLeft>
                            <PaddingRight>2pt</PaddingRight>
                            <PaddingTop>2pt</PaddingTop>
                            <PaddingBottom>2pt</PaddingBottom>
                            <Border>
                              <Color>LightGrey</Color>
                              <Style>Solid</Style>
                            </Border>
                          </Style>
                          <CanGrow>true</CanGrow>
                          <KeepTogether>true</KeepTogether>
                          <Paragraphs>
                            <Paragraph>
                              <TextRuns>
                                <TextRun>
                                  <Value>user_id</Value>
                                  <Style>
                                    <FontFamily>Arial</FontFamily>
                                    <Color>black</Color>
                                  </Style>
                                </TextRun>
                              </TextRuns>
                              <Style>
                                <FontFamily>Arial</FontFamily>
                              </Style>
                            </Paragraph>
                          </Paragraphs>
                        </Textbox>
                        <ColSpan>1</ColSpan>
                        <RowSpan>1</RowSpan>
                      </CellContents>
                    </TablixCell>
                    <TablixCell>
                      <CellContents>
                        <Textbox Name="TextBox10">
                          <Left>0in</Left>
                          <Top>0in</Top>
                          <Height>51.75pt</Height>
                          <Width>108pt</Width>
                          <Style>
                            <FontFamily>Arial</FontFamily>
                            <PaddingLeft>2pt</PaddingLeft>
                            <PaddingRight>2pt</PaddingRight>
                            <PaddingTop>2pt</PaddingTop>
                            <PaddingBottom>2pt</PaddingBottom>
                            <Border>
                              <Color>LightGrey</Color>
                              <Style>Solid</Style>
                            </Border>
                          </Style>
                          <CanGrow>true</CanGrow>
                          <KeepTogether>true</KeepTogether>
                          <Paragraphs>
                            <Paragraph>
                              <TextRuns>
                                <TextRun>
                                  <Value>user_name</Value>
                                  <Style>
                                    <FontFamily>Arial</FontFamily>
                                    <Color>black</Color>
                                  </Style>
                                </TextRun>
                              </TextRuns>
                              <Style>
                                <FontFamily>Arial</FontFamily>
                              </Style>
                            </Paragraph>
                          </Paragraphs>
                        </Textbox>
                        <ColSpan>1</ColSpan>
                        <RowSpan>1</RowSpan>
                      </CellContents>
                    </TablixCell>
                    <TablixCell>
                      <CellContents>
                        <Textbox Name="TextBox11">
                          <Left>0in</Left>
                          <Top>0in</Top>
                          <Height>51.75pt</Height>
                          <Width>108pt</Width>
                          <Style>
                            <FontFamily>Arial</FontFamily>
                            <PaddingLeft>2pt</PaddingLeft>
                            <PaddingRight>2pt</PaddingRight>
                            <PaddingTop>2pt</PaddingTop>
                            <PaddingBottom>2pt</PaddingBottom>
                            <Border>
                              <Color>LightGrey</Color>
                              <Style>Solid</Style>
                            </Border>
                          </Style>
                          <CanGrow>true</CanGrow>
                          <KeepTogether>true</KeepTogether>
                          <Paragraphs>
                            <Paragraph>
                              <TextRuns>
                                <TextRun>
                                  <Value>agent_id</Value>
                                  <Style>
                                    <FontFamily>Arial</FontFamily>
                                    <Color>black</Color>
                                  </Style>
                                </TextRun>
                              </TextRuns>
                              <Style>
                                <FontFamily>Arial</FontFamily>
                              </Style>
                            </Paragraph>
                          </Paragraphs>
                        </Textbox>
                        <ColSpan>1</ColSpan>
                        <RowSpan>1</RowSpan>
                      </CellContents>
                    </TablixCell>
                  </TablixCells>
                </TablixRow>
                <TablixRow>
                  <Height>51.75pt</Height>
                  <TablixCells>
                    <TablixCell>
                      <CellContents>
                        <Textbox Name="TextBox12">
                          <Left>0in</Left>
                          <Top>0in</Top>
                          <Height>51.75pt</Height>
                          <Width>108pt</Width>
                          <Style>
                            <FontFamily>Arial</FontFamily>
                            <PaddingLeft>2pt</PaddingLeft>
                            <PaddingRight>2pt</PaddingRight>
                            <PaddingTop>2pt</PaddingTop>
                            <PaddingBottom>2pt</PaddingBottom>
                            <Border>
                              <Color>LightGrey</Color>
                              <Style>Solid</Style>
                            </Border>
                          </Style>
                          <CanGrow>true</CanGrow>
                          <KeepTogether>true</KeepTogether>
                          <Paragraphs>
                            <Paragraph>
                              <TextRuns>
                                <TextRun>
                                  <Value>=Fields!user_id.Value</Value>
                                  <Style>
                                    <FontFamily>Arial</FontFamily>
                                  </Style>
                                </TextRun>
                              </TextRuns>
                              <Style>
                                <FontFamily>Arial</FontFamily>
                              </Style>
                            </Paragraph>
                          </Paragraphs>
                        </Textbox>
                        <ColSpan>1</ColSpan>
                        <RowSpan>1</RowSpan>
                      </CellContents>
                    </TablixCell>
                    <TablixCell>
                      <CellContents>
                        <Textbox Name="TextBox13">
                          <Left>0in</Left>
                          <Top>0in</Top>
                          <Height>51.75pt</Height>
                          <Width>108pt</Width>
                          <Style>
                            <FontFamily>Arial</FontFamily>
                            <PaddingLeft>2pt</PaddingLeft>
                            <PaddingRight>2pt</PaddingRight>
                            <PaddingTop>2pt</PaddingTop>
                            <PaddingBottom>2pt</PaddingBottom>
                            <Border>
                              <Color>LightGrey</Color>
                              <Style>Solid</Style>
                            </Border>
                          </Style>
                          <CanGrow>true</CanGrow>
                          <KeepTogether>true</KeepTogether>
                          <Paragraphs>
                            <Paragraph>
                              <TextRuns>
                                <TextRun>
                                  <Value>=Fields!user_name.Value</Value>
                                  <Style>
                                    <FontFamily>Arial</FontFamily>
                                  </Style>
                                </TextRun>
                              </TextRuns>
                              <Style>
                                <FontFamily>Arial</FontFamily>
                              </Style>
                            </Paragraph>
                          </Paragraphs>
                        </Textbox>
                        <ColSpan>1</ColSpan>
                        <RowSpan>1</RowSpan>
                      </CellContents>
                    </TablixCell>
                    <TablixCell>
                      <CellContents>
                        <Textbox Name="TextBox14">
                          <Left>0in</Left>
                          <Top>0in</Top>
                          <Height>51.75pt</Height>
                          <Width>108pt</Width>
                          <Style>
                            <FontFamily>Arial</FontFamily>
                            <PaddingLeft>2pt</PaddingLeft>
                            <PaddingRight>2pt</PaddingRight>
                            <PaddingTop>2pt</PaddingTop>
                            <PaddingBottom>2pt</PaddingBottom>
                            <Border>
                              <Color>LightGrey</Color>
                              <Style>Solid</Style>
                            </Border>
                          </Style>
                          <CanGrow>true</CanGrow>
                          <KeepTogether>true</KeepTogether>
                          <Paragraphs>
                            <Paragraph>
                              <TextRuns>
                                <TextRun>
                                  <Value>=Fields!agent_id.Value</Value>
                                  <Style>
                                    <FontFamily>Arial</FontFamily>
                                  </Style>
                                </TextRun>
                              </TextRuns>
                              <Style>
                                <FontFamily>Arial</FontFamily>
                              </Style>
                            </Paragraph>
                          </Paragraphs>
                        </Textbox>
                        <ColSpan>1</ColSpan>
                        <RowSpan>1</RowSpan>
                      </CellContents>
                    </TablixCell>
                  </TablixCells>
                </TablixRow>
              </TablixRows>
            </TablixBody>
            <TablixColumnHierarchy>
              <TablixMembers>
                <TablixMember />
                <TablixMember />
                <TablixMember />
              </TablixMembers>
            </TablixColumnHierarchy>
            <TablixRowHierarchy>
              <TablixMembers>
                <TablixMember>
                  <KeepWithGroup>After</KeepWithGroup>
                </TablixMember>
                <TablixMember>
                  <Group Name="Details1" />
                </TablixMember>
              </TablixMembers>
            </TablixRowHierarchy>
          </Tablix>
        </ReportItems>
        <Height>3.125in</Height>
      </Body>
      <Width>498.75pt</Width>
      <Page>
        <PageHeader>
          <Style>
            <FontFamily>Arial</FontFamily>
            <Border>
              <Style>None</Style>
            </Border>
          </Style>
          <Height>0.72917in</Height>
          <PrintOnFirstPage>true</PrintOnFirstPage>
          <PrintOnLastPage>true</PrintOnLastPage>
        </PageHeader>
        <PageFooter>
          <Style>
            <FontFamily>Arial</FontFamily>
            <Border>
              <Style>None</Style>
            </Border>
          </Style>
          <Height>0.72917in</Height>
          <PrintOnFirstPage>true</PrintOnFirstPage>
          <PrintOnLastPage>true</PrintOnLastPage>
        </PageFooter>
        <LeftMargin>1in</LeftMargin>
        <RightMargin>1in</RightMargin>
        <TopMargin>1in</TopMargin>
        <BottomMargin>1in</BottomMargin>
        <Style>
          <FontFamily>Arial</FontFamily>
          <Border>
            <Style>None</Style>
          </Border>
        </Style>
      </Page>
    </ReportSection>
  </ReportSections>
  <AutoRefresh>0</AutoRefresh>
  <DataSources>
    <DataSource Name="cwb">
      <ConnectionProperties>
        <DataProvider>WebAPI</DataProvider>
        <ConnectString>{"MethodType":"Get","SecurityType":"None","URL":"https://opendata.cwb.gov.tw/api/v1/rest/datastore/F-D0047-065?Authorization=CWB-AAE986FA-FC20-4F92-B4E1-6D70D0785E97&amp;format=JSON&amp;locationName=苓雅區","IsCSVFirstRowHeader":true,"DataFormat":"JSON","Separator":"comma","Delimiter":""}</ConnectString>
      </ConnectionProperties>
      <rd:ImpersonateUser>false</rd:ImpersonateUser>
    </DataSource>
    <DataSource Name="DataSource1">
      <ConnectionProperties>
        <DataProvider>WebAPI</DataProvider>
        <ConnectString>{"MethodType":"Get","SecurityType":"None","URL":"http://192.168.25.58:8115/api/public/holidayAgent/findAll","IsCSVFirstRowHeader":true,"DataFormat":"JSON","Separator":"comma","Delimiter":""}</ConnectString>
      </ConnectionProperties>
      <rd:ImpersonateUser>false</rd:ImpersonateUser>
    </DataSource>
  </DataSources>
  <DataSets>
    <DataSet Name="cwb">
      <Fields>
        <Field Name="success">
          <DataField>success</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="resource_id">
          <DataField>resource_id</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="id">
          <DataField>id</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="type">
          <DataField>type</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="datasetDescription">
          <DataField>datasetDescription</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="locationsName">
          <DataField>locationsName</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="dataid">
          <DataField>dataid</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="locationName">
          <DataField>locationName</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="geocode">
          <DataField>geocode</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="lat">
          <DataField>lat</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="lon">
          <DataField>lon</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="elementName">
          <DataField>elementName</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="description">
          <DataField>description</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="startTime">
          <DataField>startTime</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="endTime">
          <DataField>endTime</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="value">
          <DataField>value</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="measures">
          <DataField>measures</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="dataTime">
          <DataField>dataTime</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
      </Fields>
      <Query>
        <DataSourceName>cwb</DataSourceName>
        <CommandType>Text</CommandType>
        <CommandText>{"Name":"F-D0047-065","Columns":[]}</CommandText>
        <QueryDesignerState xmlns="http://schemas.microsoft.com/ReportingServices/QueryDefinition/Relational">
          <Tables>
            <Table Name="F-D0047-065" Schema="">
              <Columns>
                <Column Name="success" IsDuplicate="False" IsSelected="True" />
                <Column Name="resource_id" IsDuplicate="False" IsSelected="True" />
                <Column Name="id" IsDuplicate="False" IsSelected="True" />
                <Column Name="type" IsDuplicate="False" IsSelected="True" />
                <Column Name="datasetDescription" IsDuplicate="False" IsSelected="True" />
                <Column Name="locationsName" IsDuplicate="False" IsSelected="True" />
                <Column Name="dataid" IsDuplicate="False" IsSelected="True" />
                <Column Name="locationName" IsDuplicate="False" IsSelected="True" />
                <Column Name="geocode" IsDuplicate="False" IsSelected="True" />
                <Column Name="lat" IsDuplicate="False" IsSelected="True" />
                <Column Name="lon" IsDuplicate="False" IsSelected="True" />
                <Column Name="elementName" IsDuplicate="False" IsSelected="True" />
                <Column Name="description" IsDuplicate="False" IsSelected="True" />
                <Column Name="startTime" IsDuplicate="False" IsSelected="True" />
                <Column Name="endTime" IsDuplicate="False" IsSelected="True" />
                <Column Name="value" IsDuplicate="False" IsSelected="True" />
                <Column Name="measures" IsDuplicate="False" IsSelected="True" />
                <Column Name="dataTime" IsDuplicate="False" IsSelected="True" />
              </Columns>
              <SchemaLevels>
                <SchemaInfo Name="F-D0047-065" SchemaType="Table" />
              </SchemaLevels>
            </Table>
          </Tables>
        </QueryDesignerState>
      </Query>
    </DataSet>
    <DataSet Name="DataSet1">
      <Fields>
        <Field Name="code">
          <DataField>code</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="tw_date">
          <DataField>tw_date</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="tw_time">
          <DataField>tw_time</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="tw_day">
          <DataField>tw_day</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="user_id">
          <DataField>user_id</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="user_name">
          <DataField>user_name</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="agent_id">
          <DataField>agent_id</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="agent_name">
          <DataField>agent_name</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
        <Field Name="part_type">
          <DataField>part_type</DataField>
          <rd:TypeName>System.String</rd:TypeName>
        </Field>
      </Fields>
      <Query>
        <DataSourceName>DataSource1</DataSourceName>
        <CommandType>Text</CommandType>
        <CommandText>{"Name":"findAll","Columns":[]}</CommandText>
        <QueryDesignerState xmlns="http://schemas.microsoft.com/ReportingServices/QueryDefinition/Relational">
          <Tables>
            <Table Name="findAll" Schema="">
              <Columns>
                <Column Name="code" IsDuplicate="False" IsSelected="True" />
                <Column Name="tw_date" IsDuplicate="False" IsSelected="True" />
                <Column Name="tw_time" IsDuplicate="False" IsSelected="True" />
                <Column Name="tw_day" IsDuplicate="False" IsSelected="True" />
                <Column Name="user_id" IsDuplicate="False" IsSelected="True" />
                <Column Name="user_name" IsDuplicate="False" IsSelected="True" />
                <Column Name="agent_id" IsDuplicate="False" IsSelected="True" />
                <Column Name="agent_name" IsDuplicate="False" IsSelected="True" />
                <Column Name="part_type" IsDuplicate="False" IsSelected="True" />
              </Columns>
              <SchemaLevels>
                <SchemaInfo Name="findAll" SchemaType="Table" />
              </SchemaLevels>
            </Table>
          </Tables>
        </QueryDesignerState>
      </Query>
    </DataSet>
  </DataSets>
  <ReportParametersLayout>
    <GridLayoutDefinition>
      <NumberOfColumns>4</NumberOfColumns>
      <NumberOfRows>2</NumberOfRows>
    </GridLayoutDefinition>
  </ReportParametersLayout>
  <rd:ReportUnitType>Inch</rd:ReportUnitType>
  <rd:PageUnit>Px</rd:PageUnit>
  <df:DefaultFontFamily>Segoe UI</df:DefaultFontFamily>
</Report>
```


